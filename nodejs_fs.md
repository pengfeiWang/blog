#nodejs fs

```
fs.readdir(process.cwd(), function(err, files) {
    if (!files.length) {
        return console.log(' 空  ');
    }
    var i = 0, len = files.length;
    for( ; i < len; i++ ) {
    	console.log( process.cwd() + '/' + files[i] );
    	(function (k){
    		fs.stat( process.cwd()+ '/' + files[k], function(err, stat) {
    			console.log( process.cwd()+ '/' + files[k] )
	    		console.log( stat.isDirectory() )
	    	});
    	})(i);
    	
    }    
});
```