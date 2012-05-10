#Cassette Express

##Status

*Not Ready for Use* Most of the internal functionality complete, but interface with Express... not. The only reason this repo is public is because I'm too cheap to pay for private repos. 

##Background

I'm in the process of porting a portion of Andrew Davey's Cassette package(https://github.com/andrewdavey/cassette) to Node. It's an asset management system. This version will only deal with client side Javascript assets at this stage.

The workflow for Cassette is quite simple: 
- Break your client-side javascript code into smaller, easier to manage chunks. 
- Add references to any dependencies in your files - jQuery, Underscore, Ender, whatever you're using. 
- Request a javascript file in your template, and Cassette will automatically generate a 'bundle' which will contain all the dependencies, in the correct order. 
- In production mode, it'll take that bundle, merge it into a single file and minify the bugger to death. 

You don't need to worry about script tags. You don't need any additional client side javascript. You don't have to code your javascript in a particular style or way, and you don't need special versions of the Javascript libraries that you use all the time. 

That's Cassette. 

##Target Implementation

In app.js:

	var cassette = require('cassette-express')({
		assetsPath : process.env.PWD + '/public/javascripts',
		outputPath : '/javascripts'
	});

A little later on in app.js: 

	app.set('view options', {
		assets : cassette.middleware()
	});


In a template, i.e, layout.jade:

	!= assets.useAsset('client-app.js')

This will generate the necessary script tags to load this file, and all its dependencies, in the correct order. This saves a lot of aggro.

## Differences between Cassette-Express and Cassette MVC

- Bundles are automatically generated by requesting any particular resource inside templates. You don't need to pre-reference them.
- Bundles will always be one single file in production mode, not multiple files depending on how they were pre-referenced.
- No automagic coffeescript compiling. Poor Coffeescript.
- No stylesheet merging/compiling.
- Cassette-Express doesn't actually work yet. Hahah. 
