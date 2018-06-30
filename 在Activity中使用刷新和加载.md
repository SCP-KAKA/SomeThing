# 给一个页面添加刷新
### 1、在app下build.gradle中引入
	compile 'com.scwang.smartrefresh:SmartRefreshLayout:1.1.0-alpha-5'
    compile 'com.scwang.smartrefresh:SmartRefreshHeader:1.1.0-alpha-5'
    
### 2、在布局文件中添加刷新控件。其中包裹要刷新的内容，如下使用的RecyclerView。
	<com.scwang.smartrefresh.layout.SmartRefreshLayout
        xmlns:app="http://schemas.android.com/apk/res-auto"
        android:id="@+id/refreshLayout"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:srlEnablePreviewInEditMode="true">
        <com.scwang.smartrefresh.header.MaterialHeader
            android:layout_width="match_parent"
            android:layout_height="wrap_content"/>         
        <!-- 要刷新的部分 -->   
        <android.support.v7.widget.RecyclerView
            android:id="@+id/dynamic_list"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="#fff">
        </android.support.v7.widget.RecyclerView>        
    </com.scwang.smartrefresh.layout.SmartRefreshLayout>
    
### 3、在Activity中获取RefreshLayout控件
	//页面刷新控件
    private RefreshLayout refreshLayout;
    refreshLayout = findViewById(R.id.refreshLayout);
    
### 4、在Adapter中添加执行刷新和加载操作的方法。
	//上滑加载时执行的方法
	public void add(List<Dynamic> addMessageList){
        //增加数据
        //dataSource:adapter中的数据源
        int position = dataSource.size();
        dataSource.addAll(position,addMessageList);
        notifyItemInserted(position);//从当前加载的位置接着显示
        //在继承了BaseAdapter的自定义adapter中没有notifyItemInserted();
        //使用notifyDataSetChanged();
    }
	//下拉刷新时执行的方法
    public void refresh(List<Dynamic> newList){
        //刷新数据
        dataSource = newList;
        notifyDataSetChanged();//更新显示列表
    }
    
### 4、给refreshLayout绑定事件监听器
	refreshLayout.setOnRefreshListener(new OnRefreshListener() {
        @Override
        public void onRefresh(@NonNull RefreshLayout refreshLayout) {
            refreshLayout.finishRefresh(2000/*,false*/);
            //不传时间则立即停止刷新    传入false表示刷新失败
            //服务器请求动态数据List
              	
            //重新给Adapter添加数据
            dynamicListAdapter.refresh(list);
      
        }
    });
    refreshLayout.setOnLoadMoreListener(new OnLoadMoreListener() {
        @Override
        public void onLoadMore(@NonNull RefreshLayout refreshLayout) {
            refreshLayout.finishLoadMore(2000);
            //在这里执行上滑加载时的具体操作
            //要继续显示的数据List
            indexShopDetailAdapter.add(list);
        }
    });    
    
##### end
