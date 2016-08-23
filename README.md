# PullLoadLibrary
[ ![Download](https://api.bintray.com/packages/brokge/maven/pulltoload/images/download.svg) ](https://bintray.com/brokge/maven/pulltoload/_latestVersion)

基于 RecyclerView 封装，上拉加载更多的库。
## Usage：

### 一、GridRecyclerView
网格布局视图

```xml
<com.udaye.library.pullloadlibrary.GridRecyclerView
      android:id="@+id/recyclerview"
      android:layout_width="match_parent"
      android:layout_height="match_parent"
      android:clipToPadding="false"
      android:orientation="vertical"
      android:padding="@dimen/grid_item_spacing"
      android:scrollbarStyle="insideOverlay"
      android:scrollbars="vertical"
     />
```
Java 代码：

```java

   GridRecyclerView mRecyclerView;
   GridLayoutManager mGridLayoutManager;


   @Nullable
   @Override
   public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
       //todo init view
       mRecyclerView = (GridRecyclerView) view.findViewById(R.id.recyclerview);
       return view;
   }

   @Override
   public void onViewCreated(View view, @Nullable Bundle savedInstanceState) {
       mGridLayoutManager = new GridLayoutManager(getContext(), 3);
       mGridLayoutManager.setSpanSizeLookup(new GridLayoutManager.SpanSizeLookup() {
           @Override
           public int getSpanSize(int position) {
               switch (position % 4) {
                   case 0:
                       return 3;
                   default:
                       return 1;
               }
           }
       });
       mRecyclerView.setLayoutManager(mGridLayoutManager);
       mRecyclerView.setHasFixedSize(true);
       mRecyclerView.setOnLoadMoreListener(new OnLoadMoreListener() {
           @Override
           public void onLoadMore(SuperRecyclerView recyclerView) {
             if (isHasNext()) {
                 recyclerView.showFootProgress();
                 //requestData()
             } else {
                 recyclerView.showFootProgressEnd();
             }
                 //todo request load more    

           }
       });
       //todo other request
   }
   private boolean isHasNext(){
     //todo ishasnext

     return true;
   }

```
### 二、LinearRecyclerView
线性布局方式

```xml
<com.udaye.library.pullloadlibrary.LinearRecyclerView
            android:id="@+id/rv_recyclerview"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:clipToPadding="false"
            android:orientation="vertical"
            android:padding="@dimen/grid_item_spacing"
            android:scrollbarStyle="insideOverlay"
            android:scrollbars="vertical"
            app:spanCount="1" />


```

Java 代码：

```java

   LinearRecyclerView recyclerView;
   SwipeRefreshLayout mSwipeRefreshLayout;

   @Nullable
   @Override
   public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
       View view = inflater.inflate(R.layout.fragment_top250, null);
       recyclerView = (LinearRecyclerView) view.findViewById(R.id.rv_recyclerview);
       return view;
   }

   @Override
   public void onViewCreated(View view, @Nullable Bundle savedInstanceState) {
       LinearLayoutManager linearLayoutManager = new LinearLayoutManager(getActivity());
       recyclerView.setLayoutManager(linearLayoutManager);
       recyclerView.setHasFixedSize(true);
       recyclerView.setOnLoadMoreListener(new OnLoadMoreListener() {
           @Override
           public void onLoadMore(SuperRecyclerView recyclerView) {
               if (isHasNext()) {
                   recyclerView.showFootProgress();
                   //requestData()
               } else {
                   recyclerView.showFootProgressEnd();
               }
           }
       });
   }
private boolean isHasNext(){
  //todo ishasnext

  return true;
}


```

Adapter 适配器代码：

```java

/**
 * Created  on 16-6-6.
 */
public class CustomerAdapter extends com.udaye.library.pullloadlibrary.RecyclerViewCommonAdapter<CommonBean.SubjectsBean> {

    public CustomerAdapter(List<CommonBean.SubjectsBean> list, Context context) {
        super(context, list, R.layout.view_list_item_home);
    }

/**
*添加 list 集合
*/
    public void update(List<CommonBean.SubjectsBean> list) {
        addList((ArrayList<CommonBean.SubjectsBean>) list);
    }

    @Override
    public void onListBindViewHolder(CommonViewHolder holder, int position) {
        final CommonBean.SubjectsBean entity = mList.get(position);
        Glide.with(mContext)
                .load(mList.get(position).getImages().getLarge())
                .placeholder(android.R.color.white)
                .into((ImageView) holder.getView(R.id.photo));
        holder.setText(R.id.tv_movie_name, entity.getTitle());
        holder.itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                mContext.startActivity(MovieDetailActivity.getCallingIntent(mContext, entity.getId()));
            }
        });
    }

}
```

其中 Adapter 必须要继承  `RecyclerViewCommonAdapter` ，这是一个封装的 adapter 使用起来更方便。
