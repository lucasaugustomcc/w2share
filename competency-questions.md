# Competency questions

The following SPARQL queries can be run against the example RDF available at: <https://w3id.org/w2share/journal/example>. 

We use the following namespaces:
```sparql
base <https://w3id.org/w2share/wro/md-setup/>
prefix dcterms: <http://purl.org/dc/terms/>
prefix wf4ever: <http://purl.org/wf4ever/wf4ever#>
prefix oa:      <http://www.w3.org/ns/oa#>
prefix wfdesc: <http://purl.org/w4ever/wfdesc#>
prefix prov: <http://www.w3.org/ns/prov-o#>
prefix xsd: <http://www.w3.org/2001/XMLSchema#>
prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>
prefix ro: <http://purl.org/wf4ever/ro#> 
prefix ore: <http://www.openarchives.org/ore/terms/> 
prefix wfprov:  <http://purl.org/wf4ever/wfprov#> 
prefix foaf: <http://xmlns.com/foaf/0.1/>
```

**Query 1:** Given a software function invoked in a workflow component implementation (e.g. *ex:weka.weka3.6.2-J48Modeler*), what is the software and software version of this function?

```sparql
select ?sw ?swVersion where {
	?swVersion rdf:type sw:SoftwareVersion ;
		vff:hasSoftwareFunction ex:weka.weka3.6.2-J48Modeler .
	?sw rdf:type sw:Software ;
		sw:hasSoftwareVersion ?swVersion .
}
```

### Results
<table>
<thead>
<tr>
<th>sw</th>
<th>swVersion</th>
</tr>
</thead>
<tbody>
<tr>
<td>ex:Weka</td>
<td>ex:weka.weka3.6.2</td>
</tr></tbody></table>

**Query 2:** Are there any newer versions for a given function (e.g. *ex:weka.weka3.6.2-J48Classifier*)?

```sparql
select ?swVersionNew ?swFunctionNew where {
	?swVersionNew rdf:type sw:SoftwareVersion ;
		vff:hasSoftwareFunction ?swFunctionNew .
	?swFunctionNew rdf:type vff:SoftwareFunction ;
		prov:wasRevisionOf ex:weka.weka3.6.2-J48Classifier .
}
```

### Results
<table>
<thead>
<tr>
<th>swVersionNew</th>
<th>swFunctionNew</th>
</tr>
</thead>
<tbody>
<tr>
<td>ex:weka.weka3.9.2</td>
<td>ex:weka.weka3.9.2-J48Classifier</td>
</tr></tbody></table>

**Query 3:** What are the differences between two versions of a given software function?
In this example, we compare the query results for *ex:weka.weka3.6.2-J48Classifier* and *ex:weka.weka3.9.2-J48Classifier*. We conclude that the function in both versions has the same inputs and outputs.

```sparql
select ?inputFileName ?inputFileDataFormatName ?inputFileDataTypeName ?inputParameterName ?inputParameterDataTypeName ?outputName
	?outputDataFormatName ?outputDataTypeName where {
	ex:weka.weka3.6.2-J48Classifier rdf:type vff:SoftwareFunction ;
		vff:hasInputFile ?inputFile ;
        vff:hasInputParameter ?inputParameter ;
		vff:hasOutput ?output .
	?input vff:hasInputFileDataFormat ?inputFileDataFormat ;
		vff:hasInputFileDataType ?inputFileDataType ;
		vff:hasInputFileName ?inputFileName .
    ?inputFileDataType rdfs:label ?inputFileDataTypeName .
    ?inputFileDataFormat rdfs:label ?inputFileDataFormatName .
    ?inputParameter 
        vff:hasInputParameterDataType ?inputParameterDataType ;
		vff:hasInputParameterName ?inputParameterName .
    ?inputParameterDataType rdfs:label ?inputParameterDataTypeName .
	?output vff:hasOutputDataFormat ?outputDataFormat ;
		vff:hasOutputDataType ?outputDataType ;
		vff:hasOutputName ?outputName .
    ?outputDataType rdfs:label ?outputDataTypeName .
    ?outputDataFormat rdfs:label ?outputDataFormatName .
}
```

### Results
<table>
<thead>
<tr>
<th>inputFileName</th>
<th>inputFileDataFormatName</th>
<th>inputFileDataTypeName</th>
<th>inputParameterName</th>
<th>inputParameterDataTypeName</th>
<th>outputName</th>
<th>outputDataFormatName</th>
<th>outputDataTypeName</th>
</tr>
</thead>
<tbody>
<tr>
<td>"testFile"</td>
<td>"ARFF"</td>
<td>"Tabular"</td>
<td>"classIndex"</td>
<td>"String"</td>
<td>"classification"</td>
<td>"Java Object"</td>
<td>"Text"</td>
</tr></tbody></table>

```sparql
select ?inputFileName ?inputFileDataFormatName ?inputFileDataTypeName ?inputParameterName ?inputParameterDataTypeName ?outputName
	?outputDataFormatName ?outputDataTypeName where {
	ex:weka.weka3.9.2-J48Classifier rdf:type vff:SoftwareFunction ;
		vff:hasInputFile ?inputFile ;
        vff:hasInputParameter ?inputParameter ;
		vff:hasOutput ?output .
	?input vff:hasInputFileDataFormat ?inputFileDataFormat ;
		vff:hasInputFileDataType ?inputFileDataType ;
		vff:hasInputFileName ?inputFileName .
    ?inputFileDataType rdfs:label ?inputFileDataTypeName .
    ?inputFileDataFormat rdfs:label ?inputFileDataFormatName .
    ?inputParameter 
        vff:hasInputParameterDataType ?inputParameterDataType ;
		vff:hasInputParameterName ?inputParameterName .
    ?inputParameterDataType rdfs:label ?inputParameterDataTypeName .
	?output vff:hasOutputDataFormat ?outputDataFormat ;
		vff:hasOutputDataType ?outputDataType ;
		vff:hasOutputName ?outputName .
    ?outputDataType rdfs:label ?outputDataTypeName .
    ?outputDataFormat rdfs:label ?outputDataFormatName .
}
```

### Results
<table>
<thead>
<tr>
<th>inputFileName</th>
<th>inputFileDataFormatName</th>
<th>inputFileDataTypeName</th>
<th>inputParameterName</th>
<th>inputParameterDataTypeName</th>
<th>outputName</th>
<th>outputDataFormatName</th>
<th>outputDataTypeName</th>
</tr>
</thead>
<tbody>
<tr>
<td>"testFile"</td>
<td>"ARFF"</td>
<td>"Tabular"</td>
<td>"classIndex"</td>
<td>"String"</td>
<td>"classification"</td>
<td>"Java Object"</td>
<td>"Text"</td>
</tr></tbody></table>


**Query 4:** Retrieving lineage information associating We and script elements.

```sparql
select ?script ?abs ?process ?start ?end
where { 
   ?abs prov:wasDerivedFrom <> ;
      wfdesc:hasSubProcess ?process .
   ?process prov:wasDerivedFrom ?code .
   ?code oa:start ?start ;
      oa:end ?end .
}
```

### Results
<table>
<thead>
<tr>
<th>swFunction</th>
</tr>
</thead>
<tbody>
<tr>
<td>"ex:weka.weka3.9.2-ID3Classifier"</td>
</tr></tbody></table>

**Query 5:** Retrieving metadata about the conversion process.

```sparql
select distinct ?curator
where { 
  <abs-workflow/Setup_MD/> prov:wasAttributedTo ?agent .
  ?agent foaf:name ?curator .
}
```

### Results
<table>
<thead>
<tr>
<th>curator</th>
</tr>
</thead>
<tbody>
<tr>
<td>Lucas Carvalho</td>
</tr>
</tbody>
</table>


**Query 6:**  Retrieving information tracking workflow execution traces of We to script blocks.

```sparql
SELECT DISTINCT ?workflow ?workflowRun ?process ?output ?input
WHERE {
    ?workflow prov:wasDerivedFrom <files/Setup_MD/script.sh> .
    ?workflow wfdesc:hasSubProcess ?process .
    ?processRun wfprov:describedByProcess ?process ;
         prov:used ?input ;
         wfprov:wasPartOfWorkflowRun ?workflowRun .
    ?output prov:wasGeneratedBy ?processRun .   
}
```

### Results
<table>
<tbody>
<tr>
	<td>workflow</td><td><workflow/Setup_MD/></td></tr>
<tr>
	<td>workflowRun</td><td><run/4e0a1f-fc0f/></td>
</tr>
<tr><td>process</td><td><workflow/Setup_MD/processor/split/></td></tr>
<tr><td>input</td><td><data/4e0a1f-fc0f/input/structure.pdb></td></tr>
<tr><td>output</td><td><data/4e0a1f-fc0f/output/blgc.pdb></td></tr>
</tbody></table>

*Query 7:* Retrieving the resources available in a WRO?

```sparql
select distinct ?resource ?type
where { 
  <> ore:aggregates ?resource .
  ?resource a ?type .
  FILTER(ro:Resource != ?type)
}
```

### Results
<table>
<thead>
<tr>
<th>resource</th>
<th>type</th>
</tr>
</thead>
<tbody>
<tr>
	<td><workflow/executable-workflow.t2flow></td><td>wf4ever:Workflow</td>
	<td><workflow/refined-workflow.t2flow></td><td>wf4ever:Workflow</td>
	<td><files/script.sh></td><td>wf4ever:Script</td>
	<td><workflowrun.prov.ttl></td><td>wfdesc:WorkflowRun</td>
	<td><data/4e0a1f-fc0f/input/structure.pdb></td><td>wf4ever:Dataset</td>
	<td><data/4e0a1f-fc0f/output/blgc.pdb></td><td>wf4ever:Dataset</td>
</tr>
</tbody>
</table>