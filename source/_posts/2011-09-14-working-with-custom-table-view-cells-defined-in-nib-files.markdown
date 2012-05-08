---
layout: post
title: "Working with custom table view cells defined in NIB files"
date: 2011-09-14 08:59
comments: true
categories: [ios, iphone, tableview, tableviewcell, nib]

---

If you're using custom table view cells defined within NIB/XIB files for your iOS application, and you're a beginner like me, you may do one of these two mistakes. Just find out what not to do by having a look at this article!

<!-- more --> 

## Context

One of the projects I'm currently working on involves displaying custom UITableViewCells. To be exact, I display a dynamic list of elements which can be either displayed by a `UITableViewCell` **A** or a `UITableViewCell` **B**. Each of these cells is defined in its own NIB file.

* Cell A has 3 subviews: 2 labels and a progress view.
* Cell B has only 1 subview, the label.

## My first mistake was to try using IBOutlets to update the cell's content.

I have a subclass of `UITableViewController` which will load the cells' NIB files when needed (from `UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath` - you can have a look at the "A Closer Look at Table-View Cells" documentation page for more information).

In the cell's NIB, I have set the File's Owner as my subclass of `UITableViewController`. I setup an outlet to link to the cell so I have a reference when I load it in my table view controller. However **I also wanted to use IBOutlets to link to the cell's subviews... This is a bad idea.** This lead to some bizarre side-effects when displaying the table view, such as some rows replacing others when scrolling.

If you have subviews in your custom cell you want to update (if you don't, I'm not sure I understand how you're using your cells...), **follow the documentation and use the tag value of your subviews when editing the NIB file**. From your controller, you can access them with `[cell viewForTag:(NSInteger)tag]`.

## My second mistake was to use '0' as a tag.

One last thing. Even if I did not read it in the documentation, maybe **you should not use '0' as a tag**. I was using it for one of my subview and only the subview tagged '0' was displayed, the other disappeared. So just change the tag from the default value, and start with '1'.