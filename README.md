# Mesh 1.0 data model

This concerns the user data structure for the underlying logic system of the [live](https://immaterialAI.com) IAI app - Mesh.

## Condensed update log
### Mesh 1.1
- coming soon
### Mesh 1.0
- Node types get parentId, WorldConstraints table, Tags 
- + backup support for all new changes
### ImmaterialAI 1.X - 4.9 data model
- Contains users nodes, links (+ types), worlds and preferences, 

## Mesh implementations

### Clarity
   - coming soon

### ImmaterialAI
   - live
   - free with optional upgrade
   - cross-Platform   
   - live on Web, Android, Windows (native & Steam)
   - supports `.cdb`, `.idb`, `.txt`, `.json` and clipboard import/export
   
### ImmaterialAI Classic
   - discontinued
   - free with optional upgrade
   - native Android
   - development halted, focused changed to 3.X Cross-Platform version
   - supports `.cdb` files
   
## Contributing
If you want to contribute your own databases of concepts to this repository, open a pull request and we'll look at it.

# Database 

##### Database version 35 (latest)
Working example you can save and import [here](https://raw.githubusercontent.com/claudiu-bele/Copilot-public-data/master/neoplatonism_template.cdb)

In this version all `.cdb` files are written in JSON, both backups and public files/libraries having the same structure.

The file must be a JSON object, optionally containing arrays of objects for `users`, `nodes`, `links`, `nodeTypes`, `linkTypes`, `preferences`, `worlds` and `linkTypeNodeReqs`. The only mandatory field is `dbVersion`, the value of the database version as text, in this case "30".

Here's the available properties of each object

## Layer 0 (ground level)
### Users
They are only relevant for backups. The Users array has no reason to not be empty in public files unless you want to author a bundle as coming from a specific source.
### Nodes
Nodes are in-app concepts. 
- `id`: Text, the following are ids used by the system and are already included with the app
    - `internal_nothing` for "nothing'
    - `internal_dont_know` for "don't know"
    - `internal_everything` for "everything"
- `type`: Text matching `NodeType.id`, represents the type of concept. Below are already included with the app
    - `item_concept` for ideas
    - `item_system` for world views
    - `item_effect` for effects
    - `item_event` for events
    - `item_thought` for thoughts
    - `item_feeling` for feelings
    - `item_quote` for quotes
    - `item_study` for studies
    - `item_source` for entities
    - `internal_system` for system
    - `internal_journey` for data tied to the user journey
- `name`: Text, the name
#### Optional
- 'link' : Text, URL
- `subTitle`: Text, subtitle
- `description` : Text, can be markdown
- `imageUrl` : Text, image Url
- `link (since 3.28.1/3.3.1+)` : Text, link that can be easily accessed
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int
- `worldID (since 4.1)`: Text matching `World.id`

### Links
In-app relations.  
- `id`: Text
- `type`: Text matching `LinkType.id`, type of relation. Below are already included with the app
    - `proof` for proof/source/origin. Also uses description and data properties 
    - `parent` for parent
    - `trigger` for triggers/triggered by
    - `is` for hard equivalence
    - `analog` for softer equivalence, potentially across mutually-exclusive systems
    - `parallel` for parallels, apply your own semantic distinction factors
    - `related` for related
    - `favorite` for favorite/starred
    - `focus` for focus
    - `old_me` for new/old me distinctions
    - `todo` for task management
- `nodeId`: Text matching `Node.id`, id of the node from which the relation starts
- `userId`: Text matching `User.id`, for backups it will be the user's id, for public use "public"
#### Optional
- `otherNodeId` : Text matching `Node.id`, the id of the other node if applicable (e.g. `focus`, `favorite`, `old_me`, `todo` have no otherNodeId)
- `metaNodeId` : Text matching `Node.id`, the id of a meta node if applicable (used in analog and parallel)
- `description` : Text, meta description. For proof relations, use "obj" for objects or "boolean" and data "true" if "No proof/Nothing" is selected 
- `data` : Text, contextual data
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int


### Tag references (since Mesh 1.0)
Tag references are references to tags from any data type, supports worlds 
- `id`: Text, the following are ids used by the system and are already included with the app
- `tagId`: Text matching `Tag.id` for the tag we want to reference
- `name`: Text, the name
- `colorString`: Text, Color as a string (currently accepts ints)
- `iconUrl` : Text, icon Url
#### Optional
- `description` : Text, can be markdown
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int
- `nodeId`: Text matching `Node.id` if we want to tie the tag to a node
- `linkId`: Text matching `Link.id` if we want to tie the tag to a link
- `nodeTypeId`: Text matching `NodeType.id` if we want to tie the tag to a node type
- `linkTypeId`: Text matching `LinkType.id` if we want to tie the tag to a link type
- `worldId`: Text matching `World.id` if we want to tie the tag to a world


## Layer -1 (architecture)
### Node types
The types of which concepts/ideas can be made
- `id`: Text
- `name`: Text, the name
- `description`: Long-form text
- `colorString`: Text, Color as a string (currently accepts ints)
##### Optional
- `iconUrl` Text, icon url
- `description` : Text, can be markdown 
- (since Mesh 1.0) `parentId`: Text matching `NodeType.id`. For backups it will be the user's id, for public use "public"
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

### Link types
- `id`: Text
- `name`: Text, the name
- `sourceText`: Text on one side of the relation i.e. from A -> B what is "->"
- `description`: Text, Long-form text
- `colorString`: Text, Color as a string (currently accepts ints)
- `manyToMany`: Text, Type of relation to enable 1:1, 1:M, M:M
- `sourceIsTarget`: Boolean
##### Optional
- `targetText`: Text on the other side of the relation i.e. from B -> A what is "->"
- `sourceIconUrl` Text, icon url for the relation A -> B
- `targetIconUrl` Text, icon url for the relation B -> A
- `description` : Text, can be markdown 
- `parentLinkTypeId`: Text matching `LinkType.id`. For backups it will be the user's id, for public use "public"
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

### Worlds (since IAI 4.1)
Worlds are a way to split your library into distinct semantic worlds. 
- `id`: Text
- `name`: Text, the name
##### Optional
- `subTitle`: Text, subtitle
- `description` : Text, can be markdown
- `imageUrl` : Text, icon actually
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

### Tags (since Mesh 1.0) 
Tags are tags, not a reference to tags on individual data but the tags that are referenced themselves 
- `id`: Text, the following are ids used by the system and are already included with the app
- `name`: Text, the name
- `colorString`: Text, Color as a string (currently accepts ints)
- `iconUrl` : Text, icon Url
#### Optional
- `description` : Text, can be markdown
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

## Level -2 (constraints)

### Link Required Node Type
Constraints on what node type can be on either side of a Link Type link
- `id`: Text
- `linkTypeId`: Text matching `LinkType.id` meaning the link type to which we apply the constraint
- `nodeTypeId`: Text matching `NodeType.id` meaning the type of node we add as a requirement for a link type
- `linkTypePosition`: Text, position in which the `nodeTypeId` is added as a constraint. Can be "source", "target" or "source_target".
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

### World Constraints (since Mesh 1.0)
World constraints help further narrow down your IAI experience by only showing meta data choices based on your selected worlds' constraints. 
- `id`: Text
- `worldId`: Text matching `World.id` meaning the world to which we apply the constraint
- `type`: Text, constraint type, can be 'allow' or 'exclude'
- `metaDataType`: Text, the data type to enforce the constraint on, can be 'nodeType' or 'linkType'
- `metaDataId`: Text matching `NodeType.id` or `LinkType.id` based on `metaDataType` to represent the node/link type
- `created`: DateTime, can be string repr or int
##### Optional
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `updated`: DateTime, can be string repr or int

In the app, certain relations enforce rules as to what the type of the concepts they are connected to can be. 
Like `effect_parent`, which shows up in certain concepts as "Parent effect", and effects are the only type of item you can choose there. With that said,
those rules are not enforced when importing data, so you can write relations here that would be impossible in the app.

## Database schema changes
IAI 3 released with database version 30, featuring the structure of ImmaterialAI Classic, here's a changelog of versions since version 30
#### Db version 31 (IAI 3.28.1/ 3.3.1+)
- nullable property 'link' added to nodes
#### Db version 32 (IAI 4.1)
- added Worlds table
- nullable property 'worldId' added to nodes, points to a node


## Version 32 Example (node and link types already bundled with the app omitted)
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

