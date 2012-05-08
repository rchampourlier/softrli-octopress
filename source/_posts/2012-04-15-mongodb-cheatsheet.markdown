---
layout: post
title: "MongoDB CheatSheet"
date: 2012-04-15 10:05
comments: true
categories: [mongo, mongodb, cheatsheet, commands]

---
Another cheatsheet, for **useful MongoDB commands** to use inside the `mongo` command-line tool.

<!-- more -->

## Get a list of handy commands

    help

## List DBs

    show dbs

## Select a DB

    use mydb

## Show collections

    show collections

## List all documents in a collection

    db.collectionName.find()

## Get a specific object in a collection

### Through its index

    var object = db.collectionName.find().toArray()[index];

### With a where-like clause

Assuming you want to find objects matching x=1 and y=2:

    var objects = db.collectionName.find((x:1), (y:2));

## Update an object

    var object.attribute = newValue;
    db.collectionName.save(object);