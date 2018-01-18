### 1、 计算元音的个数 ###
	// way1
    function getCount(str) {
	  var vowelsCount = 0;
	  
	  for(let i = 0 ; i < str.length; i++){
	  	if(str[i] == 'a' || str[i] == 'e'||str[i] == 'i'|| str[i] == 'o' || str[i] == 'u')
	  	vowelsCount ++;
	  }
	  
	  return vowelsCount;
	}
	console.log(getCount("ieoiuuyuygyuuu"))
	// way2
	function getCount(str) {
	  return (str.match(/[aeiou]/ig)||[]).length;
	}
### 2、 匹配[a-m]之外出错概率###
	function printerError(s) {
    	return s.match(/[^a-m]/g).length + "/" + s.length;
	}