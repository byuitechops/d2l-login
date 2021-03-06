# d2l-login

## Description 
This tool hands over login cookies to d2l. If cookies are invalid or expired, it prompts for the username or password

## How to Install

Standard Install
    ```
    npm i --save git+https://github.com/benjameep/d2l-login.git
    ```


## How to Use
Run the following command:
```bash
node main
```

##### Handles Getting and Storing d2l cookies



``` javascript
var d2login = require('./main.js');

var d2lAPIurl = `https://byui.brightspace.com/d2l/api/lp/1.20/100283/groupcategories/`

// Using Promise Await
(async () => {

	var request = await d2login.getRequest('byui')
	
	request.get(d2lAPIurl,(err,res,body) => {
		if(err) return console.log(err)
		console.log(body)
	})
	
	var cookies = await d2login.getCookies('byui')
	console.log(cookies) // => 
/*
	[ 
		Cookie="d2lSessionVal=4KslbIcCgaYE6ggTit2NE1PAT; Path=/; hostOnly=true; aAge=1ms; cAge=1049ms",
  		Cookie="d2lSecureSessionVal=Ye9boZegghJgoJudIdPmlWLZS; Path=/; hostOnly=true; aAge=1ms; cAge=1048ms",
  		Cookie="ShibbolethSSO=LE; Path=/; hostOnly=true; aAge=1ms; cAge=1048ms" 
	]
*/
})()


// Using the callback
d2login.getRequest('byui',(err,request) => {
	if(err) return console.log(err)
	
	request.get(d2lAPIurl,(err,res,body) => {
		if(err) return console.log(err)
		console.log(body)
	})
})


```

### getRequest(subdomain, [callback])
returns the [request library](https://www.npmjs.com/package/request) which has the d2l cookies applied to it

### getCookies(subdomain, [callback])
returns an array of [tough-cookies](https://www.npmjs.com/package/tough-cookie)


### With puppeteer
``` js
async function main(){
	const cookies = await d2login.getCookies('<subdomain>');
	const browser = await puppeteer.launch({headless:false});
	const page = await browser.newPage();
	await page.setCookie(...cookies.map(c => ({
		name: c.key,
		value: c.value,
		domain: c.domain,
		path: c.path,
	})));
	await browser.close();
}
```
