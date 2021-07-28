---
layout: post
title:      "Ruby's group_by method"
date:       2021-07-28 20:33:16 +0000
permalink:  rubys_group_by_method
---


In a recent interview I was asked what now seems like one of the most basic ruby questions ever: "Given a collection of objects that have a color attribute, create a hash whose keys are the colors and whose values are a collection of objects for that color." 

The answer, which is now painfully obvious, is to use the [group_by method.](https://ruby-doc.org/core-2.6.3/Enumerable.html#method-i-group_by)

```
blocks = [{id: 1, color: "green", size: "big"}, {id: 2, color: "yellow", size: "medium"}, {id: 3, color: "black", size: "big"}, {id: 4, color: "black", size: "medium"}, {id: 5, color: "green", size: "small"}, {id: 6, color: "green", size: "small"}, {id: 7, color: "yellow", size: "big"}, {id: 8, color: "blue", size: "small"}]

puts blocks.group_by{ |h| h[:color]}

********************************************************************************************
{
     "green"=>[
		      {:id=>1, :color=>"green", :size=>"big"}, 
			  {:id=>5, :color=>"green", :size=>"small"}, 
			  {:id=>6, :color=>"green", :size=>"small"}], 
     "yellow"=>[
		       {:id=>2, :color=>"yellow", :size=>"medium"},
			   {:id=>7, :color=>"yellow", :size=>"big"}], 
     "black"=>[
		      {:id=>3, :color=>"black", :size=>"big"}, 
		      {:id=>4, :color=>"black", :size=>"medium"}], 
     "blue"=>[
		      {:id=>8, :color=>"blue", :size=>"small"}]
}
```

# How I solved it

Since I couldn't quite remember that group_by existed (and my google search for the method was poor) I tried to figure out how to do this without that nice method. 
```
#find all the unique colors

block_colors = blocks.map{ |block| block[:color] }.uniq


#make a new hash and set the block_colors array elements as the keys and the values to empty arrays

colors_hash = Hash.new(0)

block_colors.each { |color| colors_hash[color] = [] }

#find the block with the same color as the hash key and add them to the array

blocks_to_hash = blocks.each do | block |
    block_color = block[:color]
    colors_hash[block_color] << block
end

puts color_hash

********************************************************************************************
{
     "green"=>[
		      {:id=>1, :color=>"green", :size=>"big"}, 
			  {:id=>5, :color=>"green", :size=>"small"}, 
			  {:id=>6, :color=>"green", :size=>"small"}], 
     "yellow"=>[
		       {:id=>2, :color=>"yellow", :size=>"medium"},
			   {:id=>7, :color=>"yellow", :size=>"big"}], 
     "black"=>[
		      {:id=>3, :color=>"black", :size=>"big"}, 
		      {:id=>4, :color=>"black", :size=>"medium"}], 
     "blue"=>[
		      {:id=>8, :color=>"blue", :size=>"small"}]
}
```

# More information about group_by

The group_by method is cool because you have a lot of control over how information is organized by virtue of the block you're passing it. 

[Another Flatiron alum Jose Elera has an excelent blog](http://jelera.github.io/howto-work-with-ruby-group-by) on the group_by method where he looks at a lot of different nested arrays and how to organize them using [hash#transform_values](https://ruby-doc.org/core-2.6.1/Hash.html#method-i-transform_values) method among others which give you more control for more complicated data structured than the one I was given.

As with any of this, the more situations you expirement with using the group_by method the better you'll be at it. So think of some complicated data structures and get grouping!

