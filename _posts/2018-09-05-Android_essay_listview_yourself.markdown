---
layout:     post
title:      "Android随笔之自定义ListView"
subtitle:   "关于Android开发的相关知识点"
date:       2018-09-05 10:17:00
author:     "Zicon"
header-img: "img/post-bg.jpg"
catalog: true
tags:
    - Android随笔
---

> “Android自定义ListView”


# 前言

所谓的不幸运，不过是在你还没做好准备就来到你面前的机会。

抓住了就是机遇，抓不住是可惜，被反伤了就是不幸运。

---

# 正文

笔者之前一直认为自己总是缺点运气，直到前不久的一次测试吧，虽所已经提前知道了，做了一下准备，但是最终还是由于准备得不够充分，基础不行，而导致失误。

对笔者个人来说可谓是损失惨重，想必是自己得懈怠导致的后果。

 **这次记录下自定义ListView的使用吧**

自定义ListView一般适用于Item的自定义展示，比如同时含有图片文字的Item。用一个简单的例子说明就是新闻列表，既有新闻展示图还要有标题、摘要、时间。

笔者在这里整理一下自定义ListView的编写思路。
 
 **总体分支**

 - 你要展示的数据的实体类
 - 数据实体类对应的Adapter类
 - 对应实体类的ViewHolder类
 - 在Activity或Fragment中获取ListView控件并实例化
 - 实例化Adapter，ListView.setAdapter
 
 **具体设计流程**
 
 *第一步，编写Item界面*
 
首先看看每个Item的效果：
 ![](https://ZZicon.github.io/ZiconBlog/img/android_essay/ListView/news_item.png)
 
如图，Item里面要包含左边一个ImageView，右边日期的TextView，以及中间两个标题和摘要的TextView。
因此可以编写对应布局文件：list_news_item.xml

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/title_rl"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <ImageView
        android:id="@+id/news_img"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:layout_margin="18dp" />

    <TextView
        android:id="@+id/news_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginRight="60dp"
        android:layout_marginTop="20dp"
        android:layout_toRightOf="@+id/news_img"
        android:ellipsize="marquee"
        android:gravity="center"
        android:singleLine="true"
        android:textColor="@android:color/black"
        android:textSize="21sp" />

    <TextView
        android:id="@+id/news_content"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@+id/news_title"
        android:layout_marginRight="15dp"
        android:layout_marginTop="10dp"
        android:layout_toRightOf="@+id/news_img"
        android:ellipsize="end"
        android:maxLines="2"
        android:textColor="@android:color/darker_gray"
        android:textSize="17sp" />

    <TextView
        android:id="@+id/news_date"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_marginRight="15dp"
        android:layout_marginTop="20dp"
        android:textSize="18sp" />

</RelativeLayout>

```

 *第二步，编写对应的实体类*

```
public class NewsData {
    private String title;
    private long dateline;
    private String thumb;
    private String url;
    private String summary;

    public void setTitle(String title) {
        this.title = title;
    }

    public String getTitle() {
        return title;
    }

    public void setDateline(long dateline) {
        this.dateline = dateline;
    }

    public long getDateline() {
        return dateline;
    }

    public void setThumb(String thumb) {
        this.thumb = thumb;
    }

    public String getThumb() {
        return thumb;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getUrl() {
        return url;
    }

    public void setSummary(String summary) {
        this.summary = summary;
    }

    public String getSummary() {
        return summary;
    }
}
```

 *第三步，适配器适配实体类*
 
```
public class NewsAdapter extends BaseAdapter {

    private Context mContext;
    private List<NewsData> mListItems;
    private LayoutInflater mLayoutInflater;

    private SimpleDateFormat dateFormat = new SimpleDateFormat("MM-dd");

    public NewsAdapter(Context context, List<NewsData> listItems) {
        mContext = context;
        mLayoutInflater = LayoutInflater.from(context);
        mListItems = listItems;
    }

    @Override
    public int getCount() {
        return mListItems.size();
    }

    @Override
    public Object getItem(int position) {
        return mListItems.get(position);
    }

    @Override
    public long getItemId(int position) {
        return position;
    }

    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        ViewHolder holder;
		//初始化界面
        if (convertView == null) {
            holder = new ViewHolder();
            //获取布局
            convertView = mLayoutInflater.inflate(R.layout.list_news_item, null);
            //获取控件
            holder.news_img = (ImageView) convertView.findViewById(R.id.news_img);
            holder.news_title = (TextView) convertView.findViewById(R.id.news_title);
            holder.news_content = (TextView) convertView.findViewById(R.id.news_content);
            holder.news_date = (TextView) convertView.findViewById(R.id.news_date);
            //设置tag
            convertView.setTag(holder);
        } else {
            holder = (ViewHolder) convertView.getTag();
        }

        //设置相关内容
        Glide.with(mContext).load(mListItems.get(position).getThumb())
                .placeholder(R.mipmap.loading)
                .dontTransform()
                .dontAnimate().into(holder.news_img);

        // 时间戳转日期
        String sd = dateFormat.format(new Date(mListItems.get(position).getDateline() * 1000));

        holder.news_title.setText(mListItems.get(position).getTitle());
        holder.news_date.setText(sd);
        holder.news_content.setText(mListItems.get(position).getSummary());

        return convertView;
    }

    class ViewHolder {
        ImageView news_img;
        TextView news_title, news_date, news_content;
    }
}
```

 *第四步，在MainActivity布局中使用ListView*
 
 ```
 <?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ListView
        android:id="@+id/news_lv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_below="@+id/include">

    </ListView>

</RelativeLayout>

 ```
 
 *第五步，在MainActivity中实例化ListView并绑定适配器*
 
 ```
 public class MainActivity extends Activity {

    private ListView mNewsLV;
    private NewsAdapter mNewsAdapter;
    private List<NewsData> mNewsItems;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        requestWindowFeature(Window.FEATURE_NO_TITLE);
        setContentView(R.layout.activity_main);

        init();
    }

    private void init() {
        mNewsLV = (ListView) findViewById(R.id.news_lv);
		
		initDate();
		
		mNewsAdapter = new NewsAdapter(MainActivity.this, mNewsItems);
        mNewsLV.setAdapter(newsAdapter);
        mNewsAdapter.notifyDataSetChanged();
       
    }
	
	private void initDate(){
		// 获取数据
		// mNewsItems = ...
		
		// 补充一个，对list排序
		Collections.sort(newsItems, new Comparator<NewsData>() {
			/*
             * int compare(Person p1, Person p2) 返回一个基本类型的整型，
             * 返回负数表示：p1 小于p2，
             * 返回0 表示：p1和p2相等，
             * 返回正数表示：p1大于p2
             */
            @Override
            public int compare(NewsData o1, NewsData o2) {
                // 按照时间降序排序
                if (o1.getDateline() > o2.getDateline()) {
                    return -1;
                }
                if (o1.getDateline() == o2.getDateline()) {
                    return 0;
                }
                return -1;
            }
        });
	}

}
 ```

 
---

# 后记
这次的记录就完成了，为了真的让自己进步，以后笔者会尽量多做笔记的！