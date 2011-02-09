
![Alt text](https://github.com/hij1nx/Weld/raw/master/demo/public/img/weld.png)<br/>

## What is it?

Weld is like template antimatter for Javascript. It is the antithesis of most templating technology. There is no voodoo 
or special sugar required to add data into your markup. Simply, markup + instructions + data = html. And best of all it 
works in the *browser* and on your *node.js* server!

## Motivation

Most micro templating solutions require you to pepper your markup with 'stubs' or 'placeholders', which is non-standard.

- Standards compliant. No workarounds such as <%=foo%> or {foo}.
- Promote portable code/markup by decoupling desperate technologies.
- More readable code/markup.
- Increase maintainability by developers with various skill sets.

## How does it work?

<b>Usage</b>

weld can be used as a function or as a jQuery plugin. Why jQuery? A lot of people know how to use it, but mostly for the selector 
engine. But don't fret, it's a very light-weight plugin.

<pre>
  $(selector).weld(data, [config]);
</pre>
or
<pre>
  weld('.selector', data, [config]);
</pre>

The <b>data</b> parameter, an object or an array.
It's the data that you will use to populate the element collection.<br/>

The <b>config</b> parameter, an object literal (optional).

- map: function(el, key, val) { return el; } // Specify a map function to manipulate the current element.
- overwrite: true || false // append or not to the list that has already had a weld.

<pre>
  $(".contacts").weld(data, function(element, key, value) {
    $(element).append("<" + key + ">" + value + "</" + key + ">");
  });
</pre>

<b>Examples</b>

Here is some logic to create a DOM, load jQuery, read a file and Weld something...
<pre>
var fs = require(&#x27;fs&#x27;),
    jsdom = require(&#x27;jsdom&#x27;),
    html = function(file, cb) {
      
      file = __dirname + &#x27;/files/&#x27; + file;
      fs.readFile(file, function(err, data) {
        
        if (err) {
          return cb(err);
        }

        var window = jsdom.html(data.toString()).createWindow();
        jsdom.jQueryify(window, __dirname + &#x27;/../lib/jquery.js&#x27;, function() {

          window.$(&#x27;script:last&#x27;).remove();
          
          var weldTag = window.document.createElement(&#x27;script&#x27;);
          
          weldTag.src = &#x27;file://&#x27; + __dirname + &#x27;/../lib/weld.js&#x27;;
          weldTag.onload = function() {
            // remove the weld scripttag
            window.$(&#x27;script:last&#x27;).remove();
            cb(null, window.weld, window.$, window);
          };
          window.document.body.appendChild(weldTag);
        });
      })
    };
  
html('contacts.html', function(err, weld, $, window) {
  var data = [{ name: &quot;Paolo&quot;,  title : &quot;Code Slayer&quot; },
            { name: &quot;Elijah&quot;, title : &quot;Code Pimp&quot; }];

  $(&#x27;.contact&#x27;).weld(data);
});

</pre>

Here is the corresponding markup that our script above will load...
<pre>
&lt;ul class=&quot;contacts&quot;&gt;
  &lt;li class=&quot;contact&quot;&gt;
    &lt;span class=&quot;name&quot;&gt;My Name&lt;/span&gt;
    &lt;p class=&quot;title&quot;&gt;Leet Developer&lt;/p&gt;
  &lt;/li&gt;
&lt;/ul&gt;
</pre>

And here are the results that it will produce...
<pre>
&lt;ul class=&quot;contacts&quot;&gt;
  &lt;li class=&quot;contact&quot;&gt;
    &lt;span class=&quot;name&quot;&gt;Paolo&lt;/span&gt;
    &lt;p class=&quot;title&quot;&gt;Code Slayer&lt;/p&gt;
  &lt;/li&gt;
  &lt;li class=&quot;contact&quot;&gt;
    &lt;span class=&quot;name&quot;&gt;Elijah&lt;/span&gt;
    &lt;p class=&quot;title&quot;&gt;Code Pimp&lt;/p&gt;
  &lt;/li&gt;  
&lt;/ul&gt;
</pre>

## Credits
developed by tmpvar and hij1nx!!

## Version
0.1.0
