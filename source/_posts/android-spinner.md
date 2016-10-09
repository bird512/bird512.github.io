layout: post
title: 安卓下实现下拉菜单及菜单项style定制
tags: [技术, android]
comment: true
----

今天接到一个需求，要在一个原生安卓APP的list renderer里加一个下拉菜单，并且不同的下拉项要根据内容显示不同颜色的字体。自从用过react native之后再接触安卓原生感觉一下子好不习惯，在RN里短短几行就能搞定的事在原生里得改好多代码。安卓里下拉菜单控件是Spinner。直接上代码：



## 实现list renderer里的下拉菜单

先list所在的xml相应位置添加Spinner的定义：

```
<Spinner android:id="@+id/spinner1" />
```



在list adapter类里：

```
@Override
public View getView(final int position, View convertView, ViewGroup parent) {
  ViewHolder viewHolder = new ViewHolder();
  MySpinerClick mySpinerClick = null;
  if (null == convertView) {
    mySpinerClick = new MySpinerClick();
    viewHolder.spinner=(Spinner)convertView.findViewById(R.id.spinner1);
    viewHolder.spinner.setAdapter(dropdownAdapter);
    viewHolder.spinner.setSelection(dropdownAdapter.getPosition("lastValue")); //确保默认选择项
  } else {
  	...
  }
  viewHolder.spinner.setOnItemSelectedListener(mySpinerClick); //添加OnItemSelected事件
  if(null != mySpinerClick){
  	mySpinerClick.setPosition(position); //postion是什么了确保在OnItemSelect的时候能跟据这个postion找到对应的项
  }
  return convertView;
}

private final class ViewHolder {
	private Spinner spinner;
}
```



## 实现不同值显示不同样式

在Spinner的Adapter时重写getDropDownView(),这样确保了下拉弹出框里样式改变

```
final List<String> list = new ArrayList<String>();
        
        dropdownAdapter=new ArrayAdapter<String>(context,android.R.layout.simple_spinner_item,items){
            @Override
            public View getDropDownView(int position, View convertView, ViewGroup parent){
                View v = super.getDropDownView(position,convertView,parent);
                if (v != null) {
                    TextView tv = (TextView) v;
                    //改变样式
                    if(shouldDispalyRed(list.get(position))) {
                        tv.setTextColor(Color.RED);
                    }else{
                        tv.setTextColor(Color.BLACK);
                    }
                }
                return v;
            }
        };
        dropdownAdapter.setDropDownViewResource(android.R.layout.simple_spinner_dropdown_item);
```



重写Spinner的OnItemSelectedListener的onItemSelected()方法，确定在未展开状态下默认选中值的样式：

```
@Override
        public void onItemSelected(AdapterView<?> parent, View view,
                                   int pos, long id) {
            SampleBag sampleBag = sampleBags.get(position);
            sampleBag.setBagType(""+parent.getItemAtPosition(pos));
            TextView tv = ((TextView) parent.getChildAt(0));
            ////改变样式
            if(shouldDispalyRed(sampleBag.getBagType())){
                tv.setTextColor(Color.RED);
            }else{
                tv.setTextColor(Color.BLACK);
            }
        }
```

