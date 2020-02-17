# uni-app 利用 scroll-view 和 swiper 制作 顶部导航条 （可复用）

本文链接：https://blog.csdn.net/ZkD953/article/details/82968200

```
<view class="body-view">
		<scroll-view class="top-menu-view" scroll-x="true" :scroll-left="scrollLeft">
			<block v-for="(menuTab,index) in menuTabs" :key="index">
				<view class="menu-one-view" v-bind:id="'tabNum'+index" @click="swichMenu(index)">
					<view :class="[currentTab==index ? 'menu-one-act' : 'menu-one']">
						<view class="menu-one-txt">{{menuTab.name}}</view>
						<view class="menu-one-bottom">
							<view class="menu-one-bottom-color"></view>
						</view>
					</view>
				</view>
			</block>
		</scroll-view>
		<swiper :current="currentTab" class="swiper-box-list" duration="300" @change="swiperChange">
			<block v-for="(swiperDate,index1) in swiperDateList" :key="index1">
				<swiper-item>
					<scroll-view class="swiper-one-list" scroll-y="true" @scrolltolower="loadMore(index1)">
						<block v-for="(swiperDate2,index2) in swiperDate" :key="index2">
							<view class="swiper-list-entity">
								<view>{{swiperDate2}}</view>
							</view>
						</block>
					</scroll-view>
				</swiper-item>
			</block>
		</swiper>
</view>
```




	data() {
				return {
					scrollLeft: 0,
					isClickChange: false,
					currentTab: 0,
					menuTabs: [{
						name: '政策咨询'
					}, {
						name: '就业分配'
					}, {
						name: '战友互助'
					}, {
						name: '趣味杂谈'
					}, {
						name: '怀旧时光'
					}, {
						name: '军旅生活'
					}],
					swiperDateList: [[],[],[],[],[],[]]
				}
			}
	
			onLoad: function() {
				//初始化数据
				for (var i = 0; i < this.swiperDateList.length; i++) {
					this.getDateList(i);
				}
			}
	
	methods: {
				swichMenu: async function(current) { //点击其中一个 menu
						if (this.currentTab == current) {
							return false;
						} else {
							this.currentTab = current;
							this.setScrollLeft(current);
						}
					},
					swiperChange: async function(e) {
							let index = e.target.current;
							this.setScrollLeft(index);
							this.currentTab = index; 
						},
						setScrollLeft: async function(tabIndex) {
								let leftWidthSum = 0;
								for (var i = 0; i <= tabIndex; i++) {
									let nowElement = await this.getWidth('tabNum' + i);
									leftWidthSum = leftWidthSum + nowElement.width;
								}
								let winWidth = uni.getSystemInfoSync().windowWidth;
								this.scrollLeft = leftWidthSum > winWidth ? (leftWidthSum - winWidth) : 0
							},
							getWidth: function(id) { //得到元素的宽高
								return new Promise((res, rej) => {
									uni.createSelectorQuery().select("#" + id).fields({
										size: true,
										scrollOffset: true
									}, (data) => {
										res(data);
									}).exec();
								})
							},
							loadMore: function(tabIndex) {
								console.log('正在加载更多数据。。。')
								this.getDateList(tabIndex);
							},
							getDateList: function(tabIndex) {
								for (var i = 0; i < 20; i++) {
									var entity = this.menuTabs[tabIndex].name + (this.swiperDateList[tabIndex].length);
									this.swiperDateList[tabIndex].push(entity);
								}
							}
	}
```

```

	page {
			width: 100%;
			height: 100%;
			display: flex;
			flex-wrap: wrap;
			align-items: flex-start;
			justify-content: center;
			background: rgba(249, 249, 249, 1);
		}
	.body-view {
		display: flex;
		flex: 1;
		flex-direction: column;
		overflow: hidden;
		height: 100%;
		width: 100%;
		align-items: flex-start;
		justify-content: center;
	}
	 
	.top-menu-view {
		display: flex;
		white-space: nowrap;
		width: 100%;
		background-color: #FFFFFF;
		height: 112upx;
		/* 在这里设置导航条高度 */
	}
	 
	.top-menu-view .menu-one-view {
		display: inline-block;
		white-space: nowrap;
		height: 100%;
	}
	 
	.top-menu-view .menu-one-view .menu-one {
		/* 在这里写 单个按钮样式 */
		margin-left: 25upx;
		margin-right: 25upx;
		position: relative;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	 
	.top-menu-view .menu-one-view .menu-one .menu-one-txt {
		height: 40upx;
		font-size: 28upx;
		font-weight: 400;
		color: rgba(154, 154, 154, 1);
		line-height: 40upx;
	}
	 
	.top-menu-view .menu-one-view .menu-one .menu-one-bottom {
		position: absolute;
		bottom: 0;
		width: 100%;
	}
	 
	.top-menu-view .menu-one-view .menu-one .menu-one-bottom .menu-one-bottom-color {
		width: 60%;
		height: 4upx;
	}
	 
	.top-menu-view .menu-one-view .menu-one-act {
		/* 在这里写 单个按钮样式 */
		margin-left: 25upx;
		margin-right: 25upx;
		position: relative;
		height: 100%;
		display: flex;
		align-items: center;
		justify-content: center;
	}
	 
	.top-menu-view .menu-one-view .menu-one-act .menu-one-txt {
		height: 40upx;
		font-size: 28upx;
		font-weight: 400;
		color: rgba(0, 170, 255, 1);
		line-height: 40upx;
	}
	 
	.top-menu-view .menu-one-view .menu-one-act .menu-one-bottom {
		position: absolute;
		bottom: 0;
		width: 100%;
		display: flex;
		justify-content: center;
	}
	 
	.top-menu-view .menu-one-view .menu-one-act .menu-one-bottom .menu-one-bottom-color {
		width: 60%;
		height: 4upx;
		background: rgba(0, 170, 255, 1);
	}
	 
	.swiper-box-list {
		flex: 1;
		width: 100%;
		height: auto;
		background-color: #FFFFFF;
	}
	 
	.swiper-one-list {
		height: 100%;
		width: 100%;
	}
	 
	.swiper-one-list .swiper-list-entity {
		width: 100%;
		height: 112upx;
		text-align: center;
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		justify-content: center;
	}
在复用的 时候 需要 注意 一下 几点

![img](https://img-blog.csdn.net/20181008151245456?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1prRDk1Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

![img](https://img-blog.csdn.net/20181008151527787?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1prRDk1Mw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)


