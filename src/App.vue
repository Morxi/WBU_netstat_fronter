<template>
	<div id="app">
		<!-- <img src="./assets/logo.png"> -->
		<div style="padding: 10px;background: #f8f8f9">
			<Card title="WBU网络测试(非官方)" icon="ios-options" :padding="0" shadow style="">
				<CellGroup>
					<Cell title="当前位置 " :extra="Range" />
					<Cell title="点击测试当前网络" @click.native="startPing" />
					<Cell title="到学校官网" :extra="school_homepag"></Cell>
					<Cell title="到微博的延时 " :extra="weibo" />
					<Cell title="到Steam亚服延时" :extra="steam" />

					<Cell title="以下结果来自 校园网链接质量收集计划" to="https://blog.wbucs.com/index.php/archives/19/"></Cell>
					<center>
						    <Row style = "height: 20%">
						        <Col span="8">
									<i-circle :percent="80">
										<span class="demo-Circle-inner" style="font-size:24px">80%</span>
									</i-circle>
							</Col>
						<Col span="8">
									<i-circle :percent="100" stroke-color="#5cb85c">
					    <Icon type="ios-checkmark" size="60" style="color:#5cb85c"></Icon>
					</i-circle>
					</Col>
						        <Col span="8"><i-circle :percent="35" stroke-color="#ff5500">
					    <span class="demo-Circle-inner">
					        <Icon type="ios-close" size="50" style="color:#ff5500"></Icon>
					    </span></i-circle></Col>
						    </Row>
					    <Row>
					        <Col span="8">平均丢包</Col>
					        <Col span="8">网络连通性</Col>
					        <Col span="8">校园网质量</Col>
					    </Row>
					
					
					
					
					</center>
					<Cell title="正在施工中..." selected />
					<Cell title="最近24小时的校园网状态" to="/components/badge" disabled><Badge slot="extra" /></Cell>
					<!-- <Cell title="" disabled><Switch v-model="switchValue" slot="extra" /></Cell> -->
					<Cell title="" disabled />
					<Cell title="本工具由信息电子协会-开源技术部负责维护" disabled />
					<Cell title="访问我们的官网" to="https://wbucs.com" target="_blank" />
				</CellGroup>
			</Card>
		</div>
	</div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue';
import Ping from 'ping.js';
var p = new Ping();
var opt = {
	timeout: 200
};
var plo = new Ping(opt);
export default {
	data() {
		return {
			school_homepag: '',
			weibo: '',
			steam: '',
			switchValue: true,
			Range: '正在测试'
		};
	},
	mounted() {
		var that = this;
		this.$nextTick(() => {
			plo.ping('http://172.16.90.166:8900', function(err, data) {
				if (err) {
					that.Range = '校外';
				} else {
					that.Range = '校内';
				}
			});
		});
	},
	methods: {
		startPing: function(e) {
			console.log('start test');
			var that = this;
			p.ping('http://www.wbu.edu.cn/_upload/tpl/00/54/84/template84/images', function(err, data) {
				that.school_homepag = data + 'ms';
			});
			p.ping('http://weibo.com', function(err, data) {
				// Also display error if err is returned.

				that.weibo = data.toString() + 'ms';
			});
			p.ping('http://184.50.91.26', function(err, data) {
				// Also display error if err is returned.
				that.steam = data + 'ms';
				if (!data) {
					console.log('timeout');
				}
			});
		}
	},

	name: 'app',
	components: {
		HelloWorld
	}
};
</script>

<style>
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	/* text-align: center; */
	color: #2c3e50;
	width: 100vw;
	height: 100vh;
}
</style>
