---
layout: doc
title: Base Functions
categories: docs
excerpt: The metaknowledge functions, for filtering reading and writing graphs
tags: [functions]
weight: 1
---
<a name="Base Functions"></a>
The functions provided by metaknowledge are:

<ul class="post-list">
<li><article><a href="#filterNonJournals"><b>filterNonJournals</b>(<i>citesLst, invert=False</i>)</a></article></li>
<li><article><a href="#diffusionGraph"><b>diffusionGraph</b>(<i>source, target, sourceType='raw', targetType='raw'</i>)</a></article></li>
<li><article><a href="#diffusionCount"><b>diffusionCount</b>(<i>source, target, sourceType='raw', pandasFriendly=False, compareCounts=False, numAuthors=True, byYear=False</i>)</a></article></li>
<li><article><a href="#read_graph"><b>read_graph</b>(<i>edgeList, nodeList=None, directed=False, idKey='ID', eSource='From', eDest='To'</i>)</a></article></li>
<li><article><a href="#write_edgeList"><b>write_edgeList</b>(<i>grph, name, extraInfo=True, allSameAttribute=False</i>)</a></article></li>
<li><article><a href="#write_nodeAttributeFile"><b>write_nodeAttributeFile</b>(<i>grph, name, allSameAttribute=False</i>)</a></article></li>
<li><article><a href="#drop_edges"><b>drop_edges</b>(<i>grph, minWeight=-inf, maxWeight=inf, parameterName='weight', ignoreUnweighted=False, dropSelfLoops=False</i>)</a></article></li>
<li><article><a href="#drop_nodesByDegree"><b>drop_nodesByDegree</b>(<i>grph, minDegree=-inf, maxDegree=inf, useWeight=True, parameterName='weight', includeUnweighted=True</i>)</a></article></li>
<li><article><a href="#drop_nodesByCount"><b>drop_nodesByCount</b>(<i>grph, minCount=-inf, maxCount=inf, parameterName='count', ignoreMissing=False</i>)</a></article></li>
<li><article><a href="#mergeGraphs"><b>mergeGraphs</b>(<i>targetGraph, addedGraph, incrementedNodeVal='count', incrementedEdgeVal='weight'</i>)</a></article></li>
<li><article><a href="#graphStats"><b>graphStats</b>(<i>G, stats=('nodes', 'edges', 'isolates', 'loops', 'density', 'transitivity'), makeString=True</i>)</a></article></li>
<li><article><a href="#write_graph"><b>write_graph</b>(<i>grph, name, edgeInfo=True, typing=False, suffix='csv', overwrite=True</i>)</a></article></li>
<li><article><a href="#recordParser"><b>recordParser</b>(<i>paper</i>)</a></article></li>
<li><article><a href="#isiParser"><b>isiParser</b>(<i>isifile</i>)</a></article></li>
<li><article><a href="#tagToFull"><b>tagToFull</b>(<i>tag</i>)</a></article></li>
<li><article><a href="#normalizeToTag"><b>normalizeToTag</b>(<i>val</i>)</a></article></li>
<li><article><a href="#normalizeToName"><b>normalizeToName</b>(<i>val</i>)</a></article></li>
<li><article><a href="#isTagOrName"><b>isTagOrName</b>(<i>val</i>)</a></article></li>
</ul>
<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="filterNonJournals"></a><small></small>**[<ins>filterNonJournals</ins>]({{ site.baseurl }}{{ page.url }}#filterNonJournals)**(_citesLst, invert=False_):

Removes the `Citations` from _citesLst_ that are not journals

###### Parameters

_citesLst_ : `list [Citation]`

 A list of citations to be filtered

_invert_ : `optional [bool]`

 Default `False`, if `True` non-journals will be kept instead of journals

###### Returns

`list [Citation]`

 A filtered list of Citations from _citesLst_


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="diffusionGraph"></a><small></small>**[<ins>diffusionGraph</ins>]({{ site.baseurl }}{{ page.url }}#diffusionGraph)**(_source, target, sourceType='raw', targetType='raw'_):

Takes in two [`RecordCollections`]({{ site.baseurl }}{{ page.url }}#RecordCollection) and produces a graph of the citations of _source_ by the [`Records`]({{ site.baseurl }}{{ page.url }}#Record) in _target_. By default the nodes in the are `Record` objects but this can be changed with the _sourceType_ and _targetType_ keywords.

Each node on the output graph has two boolean attributes, `"source"` and `"target"` indicating if they are targets or sources. Note, if the types of the sources and targets are different the attributes will not be checked for overlap of the other type. e.g. if the source type is `'TI'` (title) and the target type is `'UT'` (WOS number), and there is some overlap of the targets and sources. Then the Record corresponding to a source node will not be checked for being one of the titles of the targets, only its WOS number will be considered.

###### Parameters

_source_ : `RecordCollection`

 A metaknowledge `RecordCollection` containing the `Records` being cited

_target_ : `RecordCollection`

 A metaknowledge `RecordCollection` containing the `Records` citing those in _source_

_sourceType_ : `str`

 default `'raw'`, if `'raw'` the returned graph will contain `Records` as source nodes. If it is a WOS tag then the nodes will be of that type.

_targetType_ : `str`

 default `'raw'`, if `'raw'` the returned graph will contain `Records` as target nodes. If it is a WOS tag of the long name of one then the nodes will be of that type.

###### Returns

`networkx Directed Graph`

 A directed graph of the diffusion network


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="diffusionCount"></a><small></small>**[<ins>diffusionCount</ins>]({{ site.baseurl }}{{ page.url }}#diffusionCount)**(_source, target, sourceType='raw', pandasFriendly=False, compareCounts=False, numAuthors=True, byYear=False_):

Takes in two [`RecordCollections`]({{ site.baseurl }}{{ page.url }}#RecordCollection) and produces a `dict` counting the citations of _source_ by the [`Records`]({{ site.baseurl }}{{ page.url }}#Record) of _target_. By default the `dict` uses `Record` objects as keys but this can be changed with the _sourceType_ keyword to any of the WOS tags.

###### Parameters

_source_ : `RecordCollection`

 A metaknowledge `RecordCollection` containing the `Records` being cited

_target_ : `RecordCollection`

 A metaknowledge `RecordCollection` containing the `Records` citing those in _source_

_sourceType_ : `optional [str]`

 default `'raw'`, if `'raw'` the returned `dict` will contain `Records` as keys. If it is a WOS tag the keys will be of that type.

_pandasFriendly_ : `optional [bool]`

 default `False`, makes the output be a dict with two keys one `"Record"` is the list of Records ( or data type requested by _sourceType_) the other is their occurrence counts as `"Counts"`. The lists are the same length.

_compareCounts_ : `optional [bool]`

 default `False`, if `True` the diffusion analysis will be run twice, first with source and target setup like the default (global scope) then using only the source `RecordCollection` (local scope).

_byYear_ : `optional [bool]`

default `False`, if `True` the returned dictionary will have Records mapped to maps, these maps will map years ('ints') to counts. If _pandasFriendly_ is also `True` the resultant dictionary will have an additional column called `'year'`. This column will contain the year the citations occurred, in addition the Records entries will be duplicated for each year they occur in.

###### Returns

`dict[:int]`

 A dictionary with the type given by _sourceType_ as keys and integers as values.

 If _compareCounts_ is `True` the values are tuples with the first integer being the diffusion in the target and the second the diffusion in the source.

 If _pandasFriendly_ is `True` the returned dict has keys with the names of the WOS tags and lists with their values, i.e. a table with labeled columns. The counts are in the column named `"TargetCount"` and if _compareCounts_ the local count is in a column called `"SourceCount"`.


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="read_graph"></a><small></small>**[<ins>read_graph</ins>]({{ site.baseurl }}{{ page.url }}#read_graph)**(_edgeList, nodeList=None, directed=False, idKey='ID', eSource='From', eDest='To'_):

Reads the files given by _edgeList_ and _nodeList_ and creates a networkx graph for the files.

This is designed only for the files produced by metaknowledge and is meant to be the reverse of [write_graph()]({{ site.baseurl }}{{ page.url }}#write_graph), if this does not produce the desired results the networkx builtin [networkx.read_edgelist()](https://networkx.github.io/documentation/networkx-1.10/reference/generated/networkx.readwrite.edgelist.read_edgelist.html) could be tried as it is aimed at a more general usage.

The read edge list format assumes the column named _eSource_ (default `'From'`) is the source node, then the column _eDest_ (default `'To'`) givens the destination and all other columns are attributes of the edges, e.g. weight.

The read node list format assumes the column _idKey_ (default `'ID'`) is the ID of the node for the edge list and the resulting network. All other columns are considered attributes of the node, e.g. count.

**Note**: If the names of the columns do not match those given to **read_graph()** a `KeyError` exception will be raised.

**Note**: If nodes appear in the edgelist but not the nodeList they will be created silently with no attributes.

###### Parameters

_edgeList_ : `str`

 a string giving the path to the edge list file

_nodeList_ : `optional [str]`

 default `None`, a string giving the path to the node list file

_directed_ : `optional [bool]`

 default `False`, if `True` the produced network is directed from _eSource_ to _eDest_

_idKey_ : `optional [str]`

 default `'ID'`, the name of the ID column in the node list

_eSource_ : `optional [str]`

 default `'From'`, the name of the source column in the edge list

_eDest_ : `optional [str]`

 default `'To'`, the name of the destination column in the edge list

###### Returns

`networkx Graph`

 the graph described by the input files


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="write_edgeList"></a><small></small>**[<ins>write_edgeList</ins>]({{ site.baseurl }}{{ page.url }}#write_edgeList)**(_grph, name, extraInfo=True, allSameAttribute=False_):

Writes an edge list of _grph_ at the destination _name_.

The edge list has two columns for the source and destination of the edge, `'From'` and `'To'` respectively, then, if _edgeInfo_ is `True`, for each attribute of the node another column is created.

**Note**: If any edges are missing an attribute it will be left blank by default, enable _allSameAttribute_ to cause a `KeyError` to be raised.

###### Parameters

_grph_ : `networkx Graph`

 The graph to be written to _name_

_name_ : `str`

 The name of the file to be written

_edgeInfo_ : `optional [bool]`

 Default `True`, if `True` the attributes of each edge will be written

_allSameAttribute_ : `optional [bool]`

 Default `False`, if `True` all the edges must have the same attributes or an exception will be raised. If `False` the missing attributes will be left blank.


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="write_nodeAttributeFile"></a><small></small>**[<ins>write_nodeAttributeFile</ins>]({{ site.baseurl }}{{ page.url }}#write_nodeAttributeFile)**(_grph, name, allSameAttribute=False_):

Writes a node attribute list of _grph_ to the file given by the path _name_.

The node list has one column call `'ID'` with the node ids used by networkx and all other columns are the node attributes.

**Note**: If any nodes are missing an attribute it will be left blank by default, enable _allSameAttribute_ to cause a `KeyError` to be raised.

###### Parameters

_grph_ : `networkx Graph`

 The graph to be written to _name_

_name_ : `str`

 The name of the file to be written

_allSameAttribute_ : `optional [bool]`

 Default `False`, if `True` all the nodes must have the same attributes or an exception will be raised. If `False` the missing attributes will be left blank.


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="drop_edges"></a><small></small>**[<ins>drop_edges</ins>]({{ site.baseurl }}{{ page.url }}#drop_edges)**(_grph, minWeight=-inf, maxWeight=inf, parameterName='weight', ignoreUnweighted=False, dropSelfLoops=False_):

Modifies _grph_ by dropping edges whose weight is not within the inclusive bounds of _minWeight_ and _maxWeight_, i.e after running _grph_ will only have edges whose weights meet the following inequality: _minWeight_ <= edge's weight <= _maxWeight_. A `Keyerror` will be raised if the graph is unweighted unless _ignoreUnweighted_ is `True`, the weight is determined by examining the attribute _parameterName_.

**Note**: none of the default options will result in _grph_ being modified so only specify the relevant ones, e.g. `drop_edges(G, dropSelfLoops = True)` will remove only the self loops from `G`.

###### Parameters

_grph_ : `networkx Graph`

 The graph to be modified.

_minWeight_ : `optional [int or double]`

 default `-inf`, the minimum weight for an edge to be kept in the graph.

_maxWeight_ : `optional [int or double]`

 default `inf`, the maximum weight for an edge to be kept in the graph.

_parameterName_ : `optional [str]`

 default `'weight'`, key to weight field in the edge's attribute dictionary, the default is the same as networkx and metaknowledge so is likely to be correct

_ignoreUnweighted_ : `optional [bool]`

 default `False`, if `True` unweighted edges will kept

_dropSelfLoops_ : `optional [bool]`

 default `False`, if `True` self loops will be removed regardless of their weight


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="drop_nodesByDegree"></a><small></small>**[<ins>drop_nodesByDegree</ins>]({{ site.baseurl }}{{ page.url }}#drop_nodesByDegree)**(_grph, minDegree=-inf, maxDegree=inf, useWeight=True, parameterName='weight', includeUnweighted=True_):

Modifies _grph_ by dropping nodes that do not have a degree that is within inclusive bounds of _minDegree_ and _maxDegree_, i.e after running _grph_ will only have nodes whose degrees meet the following inequality: _minDegree_ <= node's degree <= _maxDegree_.

Degree is determined in two ways, the default _useWeight_ is the weight attribute of the edges to a node will be summed, the attribute's name is _parameterName_ otherwise the number of edges touching the node is used. If _includeUnweighted_ is `True` then _useWeight_ will assign a degree of 1 to unweighted edges.


###### Parameters

_grph_ : `networkx Graph`

 The graph to be modified.

_minDegree_ : `optional [int or double]`

 default `-inf`, the minimum degree for an node to be kept in the graph.

_maxDegree_ : `optional [int or double]`

 default `inf`, the maximum degree for an node to be kept in the graph.

_useWeight_ : `optional [bool]`

 default `True`, if `True` the the edge weights will be summed to get the degree, if `False` the number of edges will be used to determine the degree.

_parameterName_ : `optional [str]`

 default `'weight'`, key to weight field in the edge's attribute dictionary, the default is the same as networkx and metaknowledge so is likely to be correct.

_includeUnweighted_ : `optional [bool]`

 default `True`, if `True` edges with no weight will be considered to have a weight of 1, if `False` they will cause a `KeyError` to be raised.


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="drop_nodesByCount"></a><small></small>**[<ins>drop_nodesByCount</ins>]({{ site.baseurl }}{{ page.url }}#drop_nodesByCount)**(_grph, minCount=-inf, maxCount=inf, parameterName='count', ignoreMissing=False_):

Modifies _grph_ by dropping nodes that do not have a count that is within inclusive bounds of _minCount_ and _maxCount_, i.e after running _grph_ will only have nodes whose degrees meet the following inequality: _minCount_ <= node's degree <= _maxCount_.

Count is determined by the count attribute, _parameterName_, and if missing will result in a `KeyError` being raised. _ignoreMissing_ can be set to `True` to suppress the error.

minCount and maxCount default to negative and positive infinity respectively so without specifying either the output should be the input

###### Parameters

_grph_ : `networkx Graph`

 The graph to be modified.

_minCount_ : `optional [int or double]`

 default `-inf`, the minimum Count for an node to be kept in the graph.

_maxCount_ : `optional [int or double]`

 default `inf`, the maximum Count for an node to be kept in the graph.

_parameterName_ : `optional [str]`

 default `'count'`, key to count field in the nodes's attribute dictionary, the default is the same thoughout metaknowledge so is likely to be correct.

_ignoreMissing_ : `optional [bool]`

 default `False`, if `True` nodes missing a count will be kept in the graph instead of raising an exception


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="mergeGraphs"></a><small></small>**[<ins>mergeGraphs</ins>]({{ site.baseurl }}{{ page.url }}#mergeGraphs)**(_targetGraph, addedGraph, incrementedNodeVal='count', incrementedEdgeVal='weight'_):

A quick way of merging graphs, this is meant to be quick and is only intended for graphs generated by metaknowledge. This does not check anything and as such may cause unexpected results if the source and target were not generated by the same method.

**mergeGraphs**() will **modify** _targetGraph_ in place by adding the nodes and edges found in the second, _addedGraph_. If a node or edge exists _targetGraph_ is given precedence, but the edge and node attributes given by _incrementedNodeVal_ and incrementedEdgeVal are added instead of being overwritten.

###### Parameters

_targetGraph_ : `networkx Graph`

 the graph to be modified, it has precedence.

_addedGraph_ : `networkx Graph`

 the graph that is unmodified, it is added and does **not** have precedence.

_incrementedNodeVal_ : `optional [str]`

 default `'count'`, the name of the count attribute for the graph's nodes. When merging this attribute will be the sum of the values in the input graphs, instead of _targetGraph_'s value.

_incrementedEdgeVal_ : `optional [str]`

 default `'weight'`, the name of the weight attribute for the graph's edges. When merging this attribute will be the sum of the values in the input graphs, instead of _targetGraph_'s value.


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="graphStats"></a><small></small>**[<ins>graphStats</ins>]({{ site.baseurl }}{{ page.url }}#graphStats)**(_G, stats=('nodes', 'edges', 'isolates', 'loops', 'density', 'transitivity'), makeString=True_):

Returns a string or list containing statistics about the graph _G_.

**graphStats()** gives 6 different statistics: number of nodes, number of edges, number of isolates, number of loops, density and transitivity. The ones wanted can be given to _stats_. By default a string giving a sentence containing all the requested statistics is returned but the raw values can be accessed instead by setting _makeString_ to `False`.

###### Parameters

_G_ : `networkx Graph`

 The graph for the statistics to be determined of

_stats_ : `optional [list or tuple [str]]`

 Default `('nodes', 'edges', 'isolates', 'loops', 'density', 'transitivity')`, a list or tuple containing any number or combination of the strings:

 `"nodes"`, `"edges"`, `"isolates"`, `"loops"`, `"density"` and `"transitivity"``

 At least one occurrence of the corresponding string causes the statistics to be provided in the string output. For the non-string (tuple) output the returned tuple has the same length as the input and each output is at the same index as the string that requested it, e.g.

 `_stats_ = ("edges", "loops", "edges")`

 The return is a tuple with 2 elements the first and last of which are the number of edges and the second is the number of loops

_makeString_ : `optional [bool]`

 Default `True`, if `True` a string is returned if `False` a tuple

###### Returns

`str or tuple [float and int]`

 The type is determined by _makeString_ and the layout by _stats_
 


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="write_graph"></a><small></small>**[<ins>write_graph</ins>]({{ site.baseurl }}{{ page.url }}#write_graph)**(_grph, name, edgeInfo=True, typing=False, suffix='csv', overwrite=True_):

Writes both the edge list and the node attribute list of _grph_ to files starting with _name_.

The output files start with _name_, the file type (edgeList, nodeAttributes) then if typing is True the type of graph (directed or undirected) then the suffix, the default is as follows:

>  name_fileType.suffix

Both files are csv's with comma delimiters and double quote quoting characters. The edge list has two columns for the source and destination of the edge, `'From'` and `'To'` respectively, then, if _edgeInfo_ is `True`, for each attribute of the node another column is created. The node list has one column call "ID" with the node ids used by networkx and all other columns are the node attributes.

To read back these files use [read_graph()]({{ site.baseurl }}{{ page.url }}#read_graph) and to write only one type of lsit use [write_edgeList()]({{ site.baseurl }}{{ page.url }}#write_edgeList) or [write_nodeAttributeFile()]({{ site.baseurl }}{{ page.url }}#write_nodeAttributeFile).

**Warning**: this function will overwrite files, if they are in the way of the output, to prevent this set _overwrite_ to `False`

**Note**: If any nodes or edges are missing an attribute a `KeyError` will be raised.

###### Parameters

_grph_ : `networkx Graph`

 A networkx graph of the network to be written.

_name_ : `str`

 The start of the file name to be written, can include a path.

_edgeInfo_ : `optional [bool]`

 Default `True`, if `True` the the attributes of each edge are written to the edge list.

_typing_ : `optional [bool]`

 Default `False`, if `True` the directed ness of the graph will be added to the file names.

_suffix_ : `optional [str]`

 Default `"csv"`, the suffix of the file.

_overwrite_ : `optional [bool]`

 Default `True`, if `True` files will be overwritten silently, otherwise an `OSError` exception will be raised.


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="recordParser"></a><small></small>**[<ins>recordParser</ins>]({{ site.baseurl }}{{ page.url }}#recordParser)**(_paper_):

This is function that is used to create [`Records`]({{ site.baseurl }}{{ page.url }}#Record) from files.

**recordParser**() reads the file _paper_ until it reaches 'ER'. For each field tag it adds an entry to the returned dict with the tag as the key and a list of the entries as the value, the list has each line separately, so for the following two lines in a record:

    AF BREVIK, I
       ANICIN, B

The entry in the returned dict would be `{'AF' : ["BREVIK, I", "ANICIN, B"]}`

`Record` objects can be created with these dictionaries as the initializer.

###### Parameters

_paper_ : `file stream`

 An open file, with the current line at the beginning of the WOS record.

###### Returns

`OrderedDict[str : List[str]]`

 A dictionary mapping WOS tags to lists, the lists are of strings, each string is a line of the record associated with the tag.


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="isiParser"></a><small></small>**[<ins>isiParser</ins>]({{ site.baseurl }}{{ page.url }}#isiParser)**(_isifile_):

This is function that is used to create [`RecordCollections`]({{ site.baseurl }}{{ page.url }}#RecordCollection) from files.

**isiParser**() reads the file given by the path isifile, checks that the header is correct then reads until it reaches EF. All WOS records it encounters are parsed with [**recordParser**()]({{ site.baseurl }}{{ page.url }}#recordParser) and converted into [`Records`]({{ site.baseurl }}{{ page.url }}#Record). A list of these `Records` is returned.

`BadISIFile` is raised if an issue is found with the file.

###### Parameters

_isifile_ : `str`

 The path to the target file

###### Returns

`List[Record]`

 All the `Records` found in _isifile_


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="tagToFull"></a><small></small>**[<ins>tagToFull</ins>]({{ site.baseurl }}{{ page.url }}#tagToFull)**(_tag_):

A wrapper for [`tagToFullDict`]({{ site.baseurl }}{{ page.url }}#tagProcessing) it maps 2 character tags to their full names.

###### Parameters

_tag_: `str`

 A two character string giving the tag

###### Returns

`str`

 The full name of _tag_


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="normalizeToTag"></a><small></small>**[<ins>normalizeToTag</ins>]({{ site.baseurl }}{{ page.url }}#normalizeToTag)**(_val_):

Converts tags or full names to 2 character tags, case insensitive

###### Parameters

_val_: `str`

 A two character string giving the tag or its full name

###### Returns

`str`

 The short name of _val_


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="normalizeToName"></a><small></small>**[<ins>normalizeToName</ins>]({{ site.baseurl }}{{ page.url }}#normalizeToName)**(_val_):

Converts tags or full names to full names, case sensitive

###### Parameters

_val_: `str`

 A two character string giving the tag or its full name

###### Returns

`str`

 The full name of _val_


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="isTagOrName"></a><small></small>**[<ins>isTagOrName</ins>]({{ site.baseurl }}{{ page.url }}#isTagOrName)**(_val_):

Checks if _val_ is a tag or full name of tag if so returns `True`

###### Parameters

_val_: `str`

 A string possible forming a tag or name

###### Returns

`bool`

 `True` if _val_ is a tag or name, otherwise `False`


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="BadCitation"></a><small></small>**[<ins>BadCitation</ins>]({{ site.baseurl }}{{ page.url }}#BadCitation)**(_Warning_):

Exception thrown by Citation


<hr style="padding: 0;border: none;border-width: 3px;height: 20px;color: #333;text-align: center;border-top-style: solid;border-bottom-style: solid;">

<a name="BadISIRecord"></a><small></small>**[<ins>BadISIRecord</ins>]({{ site.baseurl }}{{ page.url }}#BadISIRecord)**(_Warning_):

Exception thrown by the [record parser](#metaknowledge.recordParser) to indicate a mis-formated record. This occurs when some component of the record does not parse. The messages will be any of:

    * _Missing field on line (line Number):(line)_, which indicates a line was to short, there should have been a tag followed by information

    * _End of file reached before ER_, which indicates the file ended before the 'ER' indicator appeared, 'ER' indicates the end of a record. This is often due to a copy and paste error.

    * _Duplicate tags in record_, which indicates the record had 2 or more lines with the same tag.

    * _Missing WOS number_, which indicates the record did not have a 'UT' tag.

Records with a BadISIRecord error are likely incomplete or the combination of two or more single records.



{% include docsFooter.md %}