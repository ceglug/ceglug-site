---
tags: [flex , make]
image:
  feature: abstract-11.jpg
comments: true
author: Prashant Anantharaman
share: true
---
Flex is a tool that is widely used in the "Design of Compilers Lab" of 6th Semester Computer Science and Engineering. 

*I am writing this post, so that one can compile in just one line.*

In general we use commands like,
{% highlight sh %}
$ flex abcd.l > abcd.c
$ gcc -o abcd abcd.c -lfl
$ ./abcd
{% endhighlight %}
But this seemed somehow redundant. For example, in the below command, if I forget to give '> abcd.c' , the file automatically gets written into lex.yy.c, and keeps getting overwritten. In general, we hardly even look at the '.c' file. 
{% highlight sh %}
$ flex abcd.l > abcd.c
{% endhighlight %}
So now, we will see how to compile in just one line. 
-----------------
* Name the file with the '.l' extension. '.l' standing for lex. For eg, 'abcd.l'
* Let us say your lex source file has the following contents, 
{% highlight c %}
%{
int c;
%}
%option noyywrap
%%
. {c++;}
%%

int main(char *argc, int argv)
{
    yylex();
    printf("\n\n%d\n\n",c);
    return 0;
}
{% endhighlight %}
* Please note that the '%option noyywrap' line is a must in all lex files, using this method of compilation. 
* Come on, its just one extra line! 
* Now we are set to compile. If your filename is abcd.l, 
{% highlight sh %}
$ make abcd
{% endhighlight %}

And the output of the above command is below, 

{% highlight sh %}
lex  -t abcd.l > abcd.c
cc    -c -o abcd.o abcd.c
cc   abcd.o   -o abcd
rm abcd.c abcd.o
{% endhighlight %}
* Now to run just type 
{% highlight sh %}
$ ./abcd
{% endhighlight %}
and, we get the code running. 

Explanation to why we add the one extra line to the source lex
---------------------
* In the output of the make command, we see that it compiles using gcc, but it does not add the '-lfl' tag that we generally add.
* Thats the reason why we need to add this one line - "%option noyywrap"
* There is a way to even avoid adding this one line to our source file, but it is only for advanced users! 

------------------------------------------
Hope this post helped. You can write to the author at 

* "prashant[dot]barca[at]gmail[dot]com", or 
* [www.facebook.com/prashant.barca](www.facebook.com/prashant.barca)