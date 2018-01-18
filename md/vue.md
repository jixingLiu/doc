### 过滤器函数filter ###
### 注册局部过滤器 ###
	// 

    <div id="app">
 	  <div>${ msg | prompt('Peter', 'Hello World!') }</div>
	</div>
	var vm = new Vue({
	  el: '#app',
	  delimiters: ['${', '}'],
	  data: {
	    price: 9999999
	  },
	  Filters: {
	    priceFormat: function(value) { // 加上 $ 字號
	      return '$' + value
	    },
	    commaFormat: function(value) { // 加上千分位符號
	      return value.toString().replace(/^(-?\d+?)((?:\d{3})+)(?=\.\d+$|$)/, function (all, pre, groupOf3Digital) {
	        return pre + groupOf3Digital.replace(/\d{3}/g, ',$&');
	      })
	    }
	  }
	});
### 注册全局过滤器 ###
	<div id="app">
	  <div>${ msg | prompt('Peter', 'Hello World!') }</div>
	</div>
    Vue.Filter('prompt', function (value, arg1, arg2) {
	  return value + ' ' + arg1 + ', ' + arg2
	})