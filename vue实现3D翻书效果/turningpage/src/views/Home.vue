<template>
	<!-- <div class="home"> -->
	<div id="box" @click="turningPage">
		<div class="page">
			<div class="front"></div>
			<div class="back"></div>
		</div>
		<div class="page2"></div>
	</div>
	<!-- </div> -->
</template>

<script>
	// @ is an alias to /src
	// import HelloWorld from '@/components/HelloWorld.vue'

	export default {
		name: 'home',
		components: {
			// HelloWorld
		},
		data() {
			return {
				bReady: true,
				iNow: 0,
				oBox: "",
				oPage: "",
				oPage2: "",
				oFront: "",
				oBack: "",
			}
		},
		methods: {
			turningPage() {

				/* 我们先获取一下需要用到的页面中的元素 */

				/* 定义一个变量this.iNow，这儿里是方便后续页面拉回与换图操作 */
				// this.iNow = 0;
				/* 这里定义一个变量，当页面点击后赋值成false,防止“麒麟臂”大神无限点击翻页。
				判断当运动完成后赋值为true */
				// this.bReady = true;
				/* 给box加一个点击事件，进行翻页 */
				// this.oBox.onclick = function(){  
				/* 我们判断下，如果上次运动没结束就不能再开启新的运动，防止“撞车” */
				if (this.bReady == false) return;
				this.bReady = false;
				/* 这里我们将this.iNow的值进行++，我因为我们翻页了，所有图自然的就会加1 */
				this.iNow++;
				/* 设置翻页的运动时间运动形式等参数 */
				this.oPage.style.transition = '1s all ease';
				/* 翻页操作 */
				this.oPage.style.WebkitTransform = 'perspective(800px) rotateY(-180deg)';
				/* 这里说明一下“webkitTransitionEnd”为运动完成之后可触发的事件 */
				this.oPage.addEventListener('webkitTransitionEnd', () => {
					/* 这里当运动完成后我们进行拉回操作，将page的运动时间等设置为none，
					   // 并将翻页角度改为原来的0° */
					this.oPage.style.transition = 'none';
					this.oPage.style.WebkitTransform = 'perspective(800px) rotateY(0deg)';
					/* 下面我就行换图操作 */
					/* box的图为this.iNow%3,为啥模3呢，因为我们有三张图，如果你有八张图就模8 */
					this.oBox.style.background = `url(${require('../assets/'+(this.iNow%3)+'.png')}) no-repeat`;
					this.oFront.style.background = `url(${require('../assets/'+(this.iNow%3)+'.png')})right top no-repeat`;
					/* 这里因为back和page2组合成同一张图，且他们比box和fron组合成的图永远多1，所以这里就是(this.iNow+1)%3 */
					this.oBack.style.background = `url(${require('../assets/'+((this.iNow+1)%3)+'.png')}) no-repeat`;
					this.oPage2.style.background = `url(${require('../assets/'+((this.iNow+1)%3)+'.png')})right top no-repeat`;
					this.bReady = true;
					/* 这里我们所有的运动都完成后就将this.bReady赋值 为true*/
				}, false);
				// };
			},

		},

		mounted() {
			this.oBox = document.getElementById('box')
			this.oPage = document.querySelector('.page')
			this.oPage2 = document.querySelector('.page2')
			this.oFront = document.querySelector('.front')
			this.oBack = document.querySelector('.back')

		}
	}
</script>
<style scoped>


	html,
	body {
		overflow: hidden;
	}

	#box {
		background: url("../assets/0.png") no-repeat;
		width: 700px;
		height: 400px;
		margin: 100px auto;
		position: relative;
	}

	#box .page {
		width: 50%;
		height: 100%;
		top: 0;
		right: 0;
		position: absolute;
		transform-style: preserve-3d;
		transform-origin: left center;
		z-index: 2;
		transform: perspective(800px) rotateY(0deg);
	}

	.page .front {
		background: url("../assets/0.png") right top no-repeat;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
		position: absolute;
		backface-visibility: hidden;
		z-index: 2;
	}

	.page .back {
		background: url("../assets/1.png") left top no-repeat;
		position: absolute;
		width: 100%;
		height: 100%;
		left: 0;
		top: 0;
		transform: scale(-1, 1);
		z-index: 1;
	}

	#box .page2 {
		width: 50%;
		height: 100%;
		top: 0;
		right: 0;
		position: absolute;
		background: url("../assets/1.png") right top no-repeat;
		z-index: 1;
	}
</style>
