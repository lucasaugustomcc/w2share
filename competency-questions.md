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

**Query 1:** Retrieving information about abstract workflow Wa derived from S.

```sparql
select ?abs ?winput ?woutput ?process ?pinput ?poutput
where { 
   ?abs prov:wasDerivedFrom <files/Setup_MD/script.sh> ;
      wfdesc:hasInput ?winput;
      wfdesc:hasOutput ?woutput ;
      wfdesc:hasSubProcess ?process .
   ?process wfdesc:hasInput ?pinput;
            wfdesc:hasOutput ?poutput .
}
```

### Results
<table>
<thead>
<tr>
<th>Variable</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr><td>abs</td><td><abs-workflow/Setup_MD></td></tr>
<tr><td>winput</td><td><abs-workflow/Setup_MD/in/structure_pdb></td></tr>
<tr><td>woutput</td><td><abs-workflow/Setup_MD/out/fixed_1_pdb></td></tr>
<tr><td>processor</td><td><abs-workflow/Setup_MD/processor/split></td></tr>
<tr><td>pinput</td><td><abs-workflow/Setup_MD/processor/split/in/initial_structure></td></tr>
<tr><td>poutput</td><td><abs-workflow/Setup_MD/processor/split/out/cbh1_pdb></td></tr>
</tbody>
</table>

**Query 2:**  Retrieving information about the executable Workflow derived from S.

```sparql
select ?workflow 
where { 
   ?workflow a wfdesc:Workflow;
         prov:wasDerivedFrom <files/Setup_MD/script.sh> .
}
```

### Results
<table>
<thead>
<tr>
<th>Variable</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<td>workflow</td><td><workflow/Setup_MD/></td>
</tr></tbody></table>

**Query 3:** Retrieving information about workflow variants derived from We.

```sparql
select ?variant 
where { 
   ?variant prov:wasDerivedFrom <workflow/Setup_MD/> .
}
```

### Results
<table>
<thead>
<tr>
<th>Variable</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
	<td>variant</td>
	<td><workflow/Setup_MD/variant/></td>
</tr></tbody></table>

**Query 4:** Retrieving lineage information associating We and script elements.

```sparql
select ?abs ?process ?start ?end
where { 
   ?abs prov:wasDerivedFrom <files/Setup_MD/script.sh> ;
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
<th>abs</th>
<th>process</th>
<th>start</th>
<th>end</th>
</tr>
</thead>
<tbody>
<tr>
<td><abs-workflow/Setup_MD/></td>
	
<td><abs-workflow/Setup_MD/processor/split/></td>
	
<td>"1644"^^xsd:Integer</td>
	
<td>"1786"^^xsd:Integer</td>
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
<th>Variable</th>
<th>Value</th>
</tr>
</thead>
<tbody>
<tr>
<th>curator</th><td>Lucas Carvalho</td>
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
<thead>
<tr>
<th>Variable</th>
<th>Value</th>
</tr>
</thead>
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
<tr><td><workflow/executable-workflow.t2flow></td><td>wf4ever:Workflow</td></tr>
<tr><td><workflow/refined-workflow.t2flow></td><td>wf4ever:Workflow</td></tr>
<tr><td><files/script.sh></td><td>wf4ever:Script</td></tr>
<tr><td><workflowrun.prov.ttl></td><td>wfdesc:WorkflowRun</td></tr>
<tr><td><data/4e0a1f-fc0f/input/structure.pdb></td><td>wf4ever:Dataset</td></tr>
<tr><td><data/4e0a1f-fc0f/output/blgc.pdb></td><td>wf4ever:Dataset</td></tr>
</tbody>
</table>