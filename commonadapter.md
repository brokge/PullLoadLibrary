公共适配器

## `RecyclerViewCommonAdapter<T> extends RecyclerView.Adapter`

添加加载更多视图是借助 `RecyclerView.Adapter` 的 `getItemViewType(int position)` 特性：区分头部 、底部、正常视图。
添加视图的方法如下：

```java
/**
 * 添加一个headview
 *
 * @param headView
 */
public void addHeadView(View headView) {
    this.headView = headView;
    notifyDataSetChanged();
}

/**
 * 添加一个尾部view
 *
 * 可以是自定义 也可以是默认的 loadmore view
 *
 * @param footView
 */
public void addFootView(View footView) {
    this.footView = footView;
    notifyDataSetChanged();
}

```
显示底部视图的方法:
```java
/**
 * 加载更多
 */
public void showFootProgress() {
    if (this.mFooterLoadView == null) {
        this.mFooterLoadView = new FooterLoadView(mContext);
    }
    this.mFooterLoadView.setState(FooterLoadView.State.Loading, true);
    addFootView(this.mFooterLoadView);
}
```
加载更多全部加载完成后，要显示另外一种布局（一般加载几页加载完了的情况下适用）
```java
/**
 * 已经加载全部
 */
public void showFootLoadEnd() {
    if (this.mFooterLoadView == null) {
        this.mFooterLoadView = new FooterLoadView(mContext);
    }
    this.mFooterLoadView.setState(FooterLoadView.State.TheEnd, true);
    addFootView(this.mFooterLoadView);
    autoHideFootView();
}
```
## CommonViewHolder

是对 ViewHolder 进行封装的一个类，可以更方便的操作视图。

```java
public CommonViewHolder setText(int id, String text) {
    ((TextView) getView(id)).setText(text);
    return this;
}

public CommonViewHolder setVisibility(int id, int visibility) {
    getView(id).setVisibility(visibility);
    return this;
}

public CommonViewHolder setImageRes(int id, int res) {
    ((ImageView) getView(id)).setImageResource(res);
    return this;
}
```
`setText(R.id.tv_view,"value")`，即可设置TextView的值。同样其它方法也同样方便。
