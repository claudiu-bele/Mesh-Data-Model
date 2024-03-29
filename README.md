# Mesh SDK data model

[Mesh 1.0 announcement post](https://store.steampowered.com/news/app/1858060/view/3668778353635769023)

This concerns the user data structure of Mesh, the underlying logic system of the [live](https://immaterialAI.com) ImmaterialAI app.

# Table of contents
- [Updates](#condensed-update-log)
  - [Mesh 1.5](#mesh-15)
  - [Mesh 1.4](#mesh-14)
  - [Mesh 1.3](#mesh-13)
  - [Mesh 1.2](#mesh-12)
  - [Mesh 1.1](#mesh-11)
  - [Mesh 1.0](#mesh-10)
  - [Pre-Mesh](#immaterialai-data-model)
- [Mesh implementations](#mesh-implementations)
  - [Clarity](#clarity)
  - [ImmaterialAI](#immaterialai)
  - [ImmaterialAI classic](#immaterialai-classic)
- [Contributing](#contributing)
- [Database](#database)
  - [Latest database version](#latest-database-version)
  - [Time](#time)  
  - [Data level](#layer-0-ground-level)
    - [Node](#node)
    - [Link](#link)
    - [Tag reference](#tagreference-since-mesh-10)
    - [Data info](#datainfo-since-mesh-12)
    - [Model reference](#modelreference-since-mesh-15)
  - [Meta data level](#layer--1-architecture)
    - [NodeType](#nodetype)
    - [LinkType](#linkType)
    - [World](#world-since-iai-41)
    - [Tag](#tag-since-mesh-10)
    - [Model](#model-since-mesh-15)
  - [Constraints level](#level--2-constraints)
    - [LinkRequiredNodeType](#LinkRequiredNodeType)
    - [WorldConstraint](#WorldConstraint-since-mesh-10)


## Condensed update log
### Mesh 1.5
- database version 53
- `Model` and `ModelReference` 
- `Node` and `Link` types get new optional property `modelId` to constrain them to only one `Model`
### Mesh 1.4
- database version 52
- `Node` supports file, imageUrl
- new default `Node` and `Link` types
### Mesh 1.3
- database version 50
- Tracking AI chat sessions as nodes of type `internal_ai_chat_session`
### Mesh 1.2
- database version 45
- `DataInfo` added for tracking chages
### Mesh 1.1
- database version 43
- `TagReference` gets `tagId`, `Tag` shortened
### Mesh 1.0
- database version 35
- `NodeType` gets parentId, 
- `WorldConstraints` + `Tag` + `TagReference` tables
- backup support for all new changes
### ImmaterialAI data model
- database version 1 - 32
- Contains `User`, `Node`, `Link` (+ types), `World` and `Preference` instances,

[*back to top*](#table-of-contents)

## Mesh implementations

### Clarity
- coming soon

### ImmaterialAI
- live
- free with optional upgrade
- cross-Platform
- live on Web, Android, Windows (native & Steam)
- supports `.mesh`, `.idb`, `.cdb`, `.txt`, `.json` and clipboard import/export

### ImmaterialAI Classic
- discontinued
- free with optional upgrade
- native Android
- development halted, focused changed to 3.X Cross-Platform version
- supports `.cdb` files

[*back to top*](#table-of-contents)

## Contributing
If you want to contribute your own databases of concepts to this repository, open a pull request and we'll look at it.

[*back to top*](#table-of-contents)

# Database
#### 53 (*Latest database version* )

In this version [here](https://raw.githubusercontent.com/claudiu-bele/Mesh-Data-Model/master/neoplatonism_template.cdb) all `.mesh` data are written in JSON, both backups and public files/libraries having the same structure.

The file must be a JSON object, optionally containing the following arrays of objects:
- since Mesh 1.5: array `models`, `modelReferences`
- since Mesh 1.2: array `dataInfos`
- since Mesh 1.0: arrays `tags`, `tagInstances`, `worldConstraints`
- from IAI 3 - 4.9: arrays `users`, `nodes`, `links`, `nodeTypes`, `linkTypes`, `preferences`, `worlds`, `linkTypeNodeReqs`, `worlds`, `worldConstraints`

The only mandatory field is `dbVersion`, the value of the database version as text, in this case "53".

[*back to top*](#table-of-contents)

#### Time

Time can be described in two ways:
- (number, preferred) milliseconds since 1970
- (text, deprecated) formatted compliant with .cdb files, e.g. "Oct 24, 2020 1:09:31 PM"

[*back to top*](#table-of-contents)


# Layer 0 (ground level)
### User
They are only relevant for backups. The Users array has no reason to not be empty in public files unless you want to author a bundle as coming from a specific source.
### Node
Nodes are in-app concepts.
- `id`: Text, the following are ids used by the system and are already included with the app
- `internal_nothing` for "nothing'
- `internal_dont_know` for "don't know"
- `internal_everything` for "everything"
- `type`: Text matching `NodeType.id`, represents the type of concept. Below are already included with the app
  - `item_state` for states (since Mesh 1.4) 
  - `item_system` for world views
  - `item_effect` for effects
  - `item_event` for events
  - `item_thought` for thoughts
  - `item_feeling` for feelings
  - `item_quote` for quotes
  - `item_study` for studies
  - `item_source` for entities  
  - `item_process` for processes (since Mesh 1.4)   
  - `item_location` for locations (since Mesh 1.4) 
  - `internal_system` for system
  - `internal_journey` for data tied to the user journey
  - `internal_ai_chat_session` (since Mesh 1.3) for AI chat sessions
  - `item_concept` for any other idea that doesn't fit existing data types, or to use when wanting to generalize a node and infer that it's an idea
- `name`: Text, the name
#### Optional
- 'link' : Text, URL
- `subTitle`: Text, subtitle
- `description` : Text, can be markdown
- `imageUrl` : Text, image Url
- `link (since IAI 3.28.1/3.3.1+)` : Text, link that can be easily accessed
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int
- `worldId` (since 4.1): Text matching `World.id`
- `modelId` (since 5.30, Mesh 1.5): Text matching `Model.id`

[*back to top*](#table-of-contents)


### Link
In-app relations.
- `id`: Text
- `type`: Text matching `LinkType.id`, type of relation. Below are already included with the app
  - `proof` for proof/source/origin. Also uses description and data properties
  - `parent` for parent
  - `trigger` for triggers/triggered by
  - `is` for hard equivalence
  - `analog` for softer equivalence, potentially across mutually-exclusive systems
  - `parallel` for parallels, apply your own semantic distinction factors
  - `favorite` for favorite/starred
  - `focus` for focus
  - `old_me` for new/old me distinctions
  - `todo` for task management
  - `related` for general relatedness when all other existing types don't fit
  - `at_location` for connecting any node to `item_location` node
- `nodeId`: Text matching `Node.id`, id of the node from which the relation starts
- `userId`: Text matching `User.id`, for backups it will be the user's id, for public use "public"
#### Optional
- `otherNodeId` : Text matching `Node.id`, the id of the other node if applicable (e.g. `focus`, `favorite`, `old_me`, `todo` have no otherNodeId)
- `metaNodeId` : Text matching `Node.id`, the id of a meta node if applicable (used in analog and parallel)
- `description` : Text, meta description. For proof relations, use "obj" for objects or "boolean" and data "true" if "No proof/Nothing" is selected
- `data` : Text, contextual data
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int
- `modelId` (since 5.30, Mesh 1.5): Text matching `Model.id`

[*back to top*](#table-of-contents)

### TagReference (since Mesh 1.0)
Tag references are references to tags from any data type, supports worlds
- `id`: Text, the following are ids used by the system and are already included with the app
- `srcTagId`: Text matching `Tag.id`. For backups it will be the user's id, for public use "public"

#### Optional
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int
- `nodeId`: Text matching `Node.id` if we want to tie the tag to a node
- `linkId`: Text matching `Link.id` if we want to tie the tag to a link
- `nodeTypeId`: Text matching `NodeType.id` if we want to tie the tag to a node type
- `linkTypeId`: Text matching `LinkType.id` if we want to tie the tag to a link type
- `worldId`: Text matching `World.id` if we want to tie the tag to a world
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

[*back to top*](#table-of-contents)


### DataInfo (since Mesh 1.2)
Used for tracking additions, updates and deletions of Node, NodeType, Link, LinkType, World and Tag
- `id`: Text
- `status`: Text, can be `added`, `updated` or `deleted`
- `dataType` : Text, can be `node`, `nodeType`, `link`, `linkType`, `world` or `tag`
- `dataId` : Text matching id of any of the types mentioned above
- `description`: Long-form text
- `colorString`: Text, Color as a string (currently accepts ints)
##### Optional
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int

[*back to top*](#table-of-contents)

### ModelReference (since Mesh 1.5)
*(more coming soon, not final)*

[*back to top*](#table-of-contents)


# Layer -1 (architecture)
### NodeType
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

[*back to top*](#table-of-contents)


### LinkType
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

[*back to top*](#table-of-contents)


### World (since IAI 4.1)
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

[*back to top*](#table-of-contents)


### Tag (since Mesh 1.0)
Tags are tags, not a reference to tags on individual data but the tags that are referenced themselves
- `id`: Text, the following are ids used by the system and are already included with the app
- `name`: Text, the name
- `colorString`: Text, Color as a string (currently accepts ints)
- `iconUrl` : Text, icon Url
#### Optional
- `subTitle`: Text, the subtitle (Mesh 1.1)
- `description` : Text, can be markdown
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

[*back to top*](#table-of-contents)


### Model (since Mesh 1.5)
*(more coming soon, not final)*

[*back to top*](#table-of-contents)


# Level -2 (constraints)
### LinkRequiredNodeType
Constraints on what node type can be on either side of a Link Type link
- `id`: Text
- `linkTypeId`: Text matching `LinkType.id` meaning the link type to which we apply the constraint
- `nodeTypeId`: Text matching `NodeType.id` meaning the type of node we add as a requirement for a link type
- `linkTypePosition`: Text, position in which the `nodeTypeId` is added as a constraint. Can be "source", "target" or "source_target".
- `userId`: Text matching `User.id`. For backups it will be the user's id, for public use "public"
- `created`: DateTime, can be string repr or int
- `updated`: DateTime, can be string repr or int

[*back to top*](#table-of-contents)


### WorldConstraint (since Mesh 1.0)
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

[*back to top*](#table-of-contents)
