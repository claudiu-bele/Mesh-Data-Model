# Copilot public data

This place is for instructions on writing your own public `.cdb` files to share and import, as well as shared public files and templates.  

## Contributing
If you want to contribute your own databases of concepts to this repository, open a pull request and we'll look at it, we don't accept subjective interpretations though.

## Structure

### Latest - Db version 17
In this version all `.cdb` files are written in JSON, both backups and public files/libraries having the same structure.

The file must be a JSON object, optionally containing arrays of objects named `users`, `nodes`, and `links`. The only mandatory field is `dbVersion`, the value of the database version as text,
in this case "17".

Here's the available properties of each object
#### Users
They are only relevant for backups, they do not represent authors. The Users array has no reason to not be empty or be there in public files.
#### Nodes
They are in-app concepts. 
- id: Text
- type: enum, represents the type of concept
    - item_concept for ideas
    - item_system for world views
    - item_effect for effects
    - item_event for events
    - item_thought for thoughts
    - item_feeling for feelings
- name: Text, the name
- userId: Text. For backups it will be the user's id, for public use "public"
##### Optional
- description : Text, can be markdown
- imageUrl : Text, image Url
- created : Text, format "Jun 26, 2020 5:12:08 PM"
- updated : Text, format "Jun 26, 2020 5:12:08 PM"

#### Links
In-app relations.  
- id: Text
- type: enum, type of relation
    - proof for proof/source/origin. Also uses description and data properties 
    - trigger for triggers/triggered by
    - analog for analogous (not supported yet)
    - parallel for parallels (not supported yet)
    - related for related
    - favorite for favorite/starred
    - focus for focus
    - {node_type}_parent variants (works for concept, system, effect, event, thought, feeling).  
- nodeId: id of the node from which the relation starts
- userId: Text, for backups it will be the user's id, for public use "public"
##### Optional
- otherNodeId : Text, the id of the other node if applicable (e.g. focus and favorite have no otherNodeId)
- metaNodeId : Text, the id of a meta node if applicable (used in analog and parallel)
- description : Text, meta description. For proof relations, use "obj" for objects or "boolean" and data "true" if "No proof/Nothing" is selected 
- data : Text, meta data. For proof relations and "No proof/Nothing" option, use "true"
- created : Text, format "Jun 26, 2020 5:12:08 PM"
- updated : Text, format "Jun 26, 2020 5:12:08 PM"

In the app, certain relations enforce rules as to what the type of the concepts they are connected to can be. 
Like `effect_parent`, which shows up in certain concepts as "Parent effect", and effects are the only type of item you can choose there. With that said,
those rules are not enforced when importing data, so you can write relations here that would be impossible in the app.

#### Version 17 Example
```
{
  "links":[
    {
      "id":"neop_component_nous",
      "nodeId":"id_nous",
      "otherNodeId":"id_neoplatonism",
      "type":"related",
      "userId":"public"
    },
    {
      "id":"neop_author",
      "nodeId":"id_neoplatonism",
      "otherNodeId":"id_plotinus",
      "type":"related",
      "userId":"public"
    }
  ],
  "nodes":[
    {
      "id":"id_neoplatonism",
      "name":"Neoplatonism",
      "type":"item_system",
      "userId":"public"
    },
    {
      "description":"",
      "id":"id_nous",
      "name":"The nous",
      "type":"item_concept",
      "userId":"public"
    },
    {
      "id":"id_plotinus",
      "name":"Plotinus",
      "description":
"## Some description.

Multiple lines? Be **bold**",
      "type":"item_concept",
      "userId":"public",
      "imageUrl":"https://upload.wikimedia.org/wikipedia/commons/e/ee/Plotinos.jpg"
    }
  ],
  "dbVersion":"17"
}
```

