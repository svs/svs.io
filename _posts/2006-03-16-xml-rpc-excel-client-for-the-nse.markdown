---
layout: post
title: XML-RPC Excel client for the NSE
date: '2006-03-16 11:24:00'
---

<p>I spent the afternoon writing an XML-RPC server and client in Excel for accessing the National Stock Exchange&rsquo;s order books. This is great if you want to just get the current bid and offer prices for various options or futures contracts in a portfolio. The RPC server scrapes the NSE site for the info and returns it as XML. The simple getquote() function in Excel is responsible for sending the request and returning the answer.</p>

<p>The sample Excel sheet is at <a href="http://patang.org/projects/nse/XMLRPCOptionViewer.xls" target="_blank">http://patang.org/projects/nse/XMLRPCOptionViewer.xls</a></p>

<p>You might have to add a reference to the comxmlrpc library in the macro code. The library is available at <a href="http://comxmlrpc.sourceforge.net/" target="_blank">http://comxmlrpc.sourceforge.net/</a>.</p>

<p>Right now the RPC server only returns the bid side quote. I&rsquo;m working on making it return more comprehensive data. If you download and use the spreadsheet, please let me know.</p>