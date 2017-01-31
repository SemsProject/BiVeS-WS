Usage of the BiVeS Web Service 
==============================

The BiVeS Web Service expects JSON object sent as a [HTTP Post](https://en.wikipedia.org/wiki/POST_%28HTTP%29) query. This JSON object needs to have the following entries:
* `files`: an array of files to be processed with this query (see [None](#files))
* `commands`: an array of command to be run on the server (see [None](#commands))

`files` 
-------

Models to be processed using BiVeS are supplied in the `files` array of the JSON object. Each element my be **a link to the model** accessible for the web service or **plain XML** code (in that case the model must not contain imports).

There are two operation modes:
* single file mode: the files array must be of size one (only submit one single file)
* comparison mode: the files array must be of size two (submit exactly two files to be compared)

see also [sample queries](#sample-queries)

`commands` 
----------

The commands array contains commands to be executed on the web server. These commands can be divided into the two operation modes of BiVeS:

* **comparison mode:**
 * `CellML`: force CellML comparison (do not try to detect file types)
 * `SBML`: force SBML comparison (do not try to detect file types)
 * `regular`: force simple XML comparison (do not try to detect file types)
 * `reactionsDot`: get the highlighted reaction network encoded in DOT language (see [ReactionNetwork](http://sems.uni-rostock.de/trac/bives-core/wiki/ReactionNetwork) and [DOT description](http://sems.uni-rostock.de/trac/bives/wiki/DotFormatDescription))
 * `reactionsGraphml`: get the highlighted reaction network encoded in GraphML (see [ReactionNetwork](http://sems.uni-rostock.de/trac/bives-core/wiki/ReactionNetwork) and [GraphML description](http://sems.uni-rostock.de/trac/bives/wiki/GraphmlFormatDescription))
 * `reactionsJson`: get the highlighted reaction network encoded in JSON (see [ReactionNetwork](http://sems.uni-rostock.de/trac/bives-core/wiki/ReactionNetwork) and [JSON description](http://sems.uni-rostock.de/trac/bives/wiki/JsonGraphFormatDescription))
 * `compHierarchyDot`: get the highlighted component hierarchy encoded in DOT language (see [Component Hierarchy](http://sems.uni-rostock.de/trac/bives-core/wiki/HierarchyNetwork) and [DOT description](http://sems.uni-rostock.de/trac/bives/wiki/DotFormatDescription))
 * `compHierarchyGraphml`: get the highlighted component hierarchy encoded in GraphML (see [Component Hierarchy](http://sems.uni-rostock.de/trac/bives-core/wiki/HierarchyNetwork) and [GraphML description](http://sems.uni-rostock.de/trac/bives/wiki/GraphmlFormatDescription))
 * `compHierarchyJson`: get the highlighted component hierarchy encoded in JSON (see [Component Hierarchy](http://sems.uni-rostock.de/trac/bives-core/wiki/HierarchyNetwork) and [JSON description](http://sems.uni-rostock.de/trac/bives/wiki/JsonGraphFormatDescription))
 * `reportHtml`: get the difference report encoded in HTML (see [HTML description](http://sems.uni-rostock.de/trac/bives-core/wiki/ReportHtml))
 * `reportMd`: get the difference report encoded in !MarkDown (see [MarkDown description](http://sems.uni-rostock.de/trac/bives-core/wiki/ReportMarkDown))
 * `reportRST`: get the difference report encoded in !re/StructuredText (see [report/ReStructuredText description](http://sems.uni-rostock.de/trac/bives-core/wiki/ReportReStructuredText))
 * `xmlDiff`: returns the delta encoded in XML (see [Delta description](http://sems.uni-rostock.de/trac/bives-core/wiki/BivesDelta))
* **single file mode:**
 * `documentType`: get the document type of a model file, simple way to classify an XML document (see [classification](http://sems.uni-rostock.de/trac/bives/wiki/BivesClassifier))
 * `meta`: get some meta data of a model (see [meta data description](http://sems.uni-rostock.de/trac/bives/wiki/BivesMeta))
 * `singleFlatten`: get the flattened document (write imported modules directly into the model) 
 * `singleReactionsDot`: get the reaction network corresponding to the provided model encoded in DOT language (see [ReactionNetwork](http://sems.uni-rostock.de/trac/bives-core/wiki/ReactionNetwork) and [DOT description](http://sems.uni-rostock.de/trac/bives/wiki/DotFormatDescription))
 * `singleReactionsGraphml`: get the reaction network corresponding to the provided model encoded in GraphML (see [ReactionNetwork](http://sems.uni-rostock.de/trac/bives-core/wiki/ReactionNetwork) and [GraphML description](http://sems.uni-rostock.de/trac/bives/wiki/GraphmlFormatDescription))
 * `singleReactionsJson`: get the reaction network corresponding to the provided model encoded in JSON (see [ReactionNetwork](http://sems.uni-rostock.de/trac/bives-core/wiki/ReactionNetwork) and [JSON description](http://sems.uni-rostock.de/trac/bives/wiki/JsonGraphFormatDescription))
 * `singleCompHierarchyDot`: get the component hierarchy encoded in DOT language (see [Component Hierarchy](http://sems.uni-rostock.de/trac/bives-core/wiki/HierarchyNetwork) and [DOT description](http://sems.uni-rostock.de/trac/bives/wiki/DotFormatDescription))
 * `singleCompHierarchyGraphml`: get the component hierarchy encoded in GraphML (see [Component Hierarchy](http://sems.uni-rostock.de/trac/bives-core/wiki/HierarchyNetwork) and [GraphML description](http://sems.uni-rostock.de/trac/bives/wiki/GraphmlFormatDescription))
 * `singleCompHierarchyJson`:get the component hierarchy encoded in JSON (see [Component Hierarchy](http://sems.uni-rostock.de/trac/bives-core/wiki/HierarchyNetwork) and [JSON description](http://sems.uni-rostock.de/trac/bives/wiki/JsonGraphFormatDescription))

(list as of version 1.2.3, to get a more up-to-date list of available commands send a GET request to the web service, i.e. access the web service using your web browser)

see also [sample queries](#sample-queries)

Result 
-------
As a result the web service returns a JSON object which contains an entry for each command holding the result of it. The key of this entry equals the command you sent to the web service. That means if you send a request such as

```
#!js
{
	"files": [...],
	"commands":
	[
		...
		"compHierarchyJson",
		...
	]
}
```

you will receive an object like:

```
#!js
{
    ...
    "compHierarchyJson": "...",
    ...
}
```

There is a special key `error` that contains errors which occurred during processing of your request (see for example [Send an invalid request](#send-an-invalid-request)).

Sample Queries 
---------------

* the following queries were run using curl
* in addition, the output is piped through `python -mjson.tool` to prettify the JSON result. feel free to drop that.
* here we use the bives web service hosted at our server: http://bives.sems.uni-rostock.de/bives/

### Get the HTML report and the reaction network after comparing two files 

```
curl -d '{
	"files":
	[
		"http://budhat.sems.uni-rostock.de/download?downloadModel=24",
		"http://budhat.sems.uni-rostock.de/download?downloadModel=25"
	],
	"commands":
	[
		"SBML",
		"reactionsDot",
		"reportHtml"
	]
}' http://bives.sems.uni-rostock.de/bives/ | python -mjson.tool
```


### Flatten a CellML model 

* read more about [flattening CellML models](http://sems.uni-rostock.de/trac/bives-cellml/wiki/FlattenModels)

```
curl -d '{
	"files":
	[
		"http://models.cellml.org/exposure/385475ef63ff3f2d42e3dcb52f3982d2/MainDVad.cellml"
	],
	"commands":
	[
		"CellML",
		"singleFlatten"
	]
}' http://bives.sems.uni-rostock.de/bives/ | python -mjson.tool
```

### Send an invalid request 

* you for example force SBML comparison but provide SBML models you'll receive an error:

```
curl -d '{
	"files":
	[
		"http://budhat.sems.uni-rostock.de/download?downloadModel=24",
		"http://budhat.sems.uni-rostock.de/download?downloadModel=25"
	],
	"commands":
	[
		"CellML",
		"compHierarchyJson",
		"reportHtml"
	]
}' http://bives.sems.uni-rostock.de/bives/ | python -mjson.tool
```

```js
{
    "error": [
        "Error: cellml document does not define a model"
    ]
}
```
