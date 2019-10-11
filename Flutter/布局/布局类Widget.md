## 介绍
布局类Widget都会包含一个或多个子widget，不同的布局类Widget对子widget排版(layout)方式不同。我们在前面说过Element树才是最终的绘制树，Element树是通过widget树来创建的（通过Widget.createElement()），widget其实就是Element的配置数据。Flutter中，根据Widget是否需要包含子节点将Widget分为了三类，分别对应三种Element，如下表：

Widget|	对应的Element	|用途
--|--|--
LeafRenderObjectWidget|	LeafRenderObjectElement|	Widget树的叶子节点，用于没有子节点的widget，通常基础widget都属于这一类，如Text、Image。
SingleChildRenderObjectWidget|	SingleChildRenderObjectElement|	包含一个子Widget，如：ConstrainedBox、DecoratedBox等
MultiChildRenderObjectWidget|	MultiChildRenderObjectElement|	包含多个子Widget，一般都有一个children参数，接受一个Widget数组。如Row、Column、Stack等

>注意，Flutter中的很多Widget是直接继承自StatelessWidget或StatefulWidget，然后在build()方法中构建真正的RenderObjectWidget，如Text，它其实是继承自StatelessWidget，然后在build()方法中通过RichText来构建其子树，而RichText才是继承自LeafRenderObjectWidget。所以为了方便叙述，我们也可以直接说Text属于LeafRenderObjectWidget（其它widget也可以这么描述），这才是本质。读到这里我们也会发现，其实StatelessWidget和StatefulWidget就是两个用于组合Widget的基类，它们本身并不关联最终的渲染对象（RenderObjectWidget）。

## 常见布局
1. 线性布局 
   > Row、Column
2. 弹性布局
   >Flex
3. 流式布局
   > Wrap、Flow
4. 层叠布局
   > Stack、Positioned