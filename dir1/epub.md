###### gepub
---



```usage.rb
require 'rubygems'
require 'gepub'

book = GEPUB::Book.new
book.primary_identifier('http://tkgcci.com/bookid_in_url', '', '')
book.language = 'ja'

book.add_title 'GEPUBtext',
                title_type: GEPUB::TITLE_TYPE::MAIN,
                lang: 'ja',
                file_as: 'GEPUB text',
                display_seq: 1,
                alternates: {
                        '' => '',
                        '' => '',
                        '' => '' }

book.add_title('TITLE', title_type: GEPUB::TITLE_)

book.add_creator '',
                 display_seq:1,
                 alternates: { 'en' => 'Takashi Yoshioka' }
book.add_contributor '',
                     display_seq: 1,
                     alternates: {'' => ''}
book.add_contributor '',
                     display_seq: 2,
                     alternates: {}

book.add_contributor().display_seq(3).add_altenates()
book.add_contributor().display_seq(4).add_alternates()

imgfile = File.join(File.dirname(), '')
File.open(imgfile) do
  |io|
  book.add_item('img/image1.jpg', content: io).cover_image
end

book.ordered {
  book.add_item('text/cp1.xhtml',
                content: StringIO.new().landmark()
                <html xmlns="http://www.w3.org/1999/xhtml">
                <>
                <>
                </html>)
}
epubname = File.join(File.dirname(__FILE__), 'test.epub')

book.generate_epub(epubname)
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```


```
```

