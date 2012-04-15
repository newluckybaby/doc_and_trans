#用于描述JSON文档结构及含义的JSON媒体类型#

##概要##
JSON(Javascript Object Notation) Schema 是对“application/schema+json”的定义，此文档类型以JSON数据格式为基础。从当前应用场景来看，JSON Schema所能做的主要是对数据形式以及操作方式进行约束，同时也包括对校验、文档、链接导航、访问控制的定义。

##文档状态##
   
此**互联网草案**以符合BCP 78与BCP 79规定的方式进行提交。

所谓**互联网草案**是指**互联网工程工作组**的工作文档，需要注意的是其他工作组也可以将此文档以**互联网草案**的形式分发。可以通过[http://datatracker.ietf.org/drafts/current/](http://datatracker.ietf.org/drafts/current/)查看当前已存在的**互联网草案**。

此文档过期时间为二零一一年五月二十六日。

##版权声明##
Copyright (c) 2010 版权归互联网工程工作组以及作者所有。

在有效期内，此文档受BCP 78以及互联网工程工作组相关文档条例[http://trustee.ietf.org/license-info](http://trustee.ietf.org/license-info)保护。请仔细阅读以上条例内容中关于权利与约束条件内容。从此文档中提取的任何代码片段需要包含简化BSD许可证中章节4.e中的内容。

##一、介绍##
JSON(Javascript Object Notation)Schema是对JSON类型数据结构的定义。从当前应用场景来看，JSON Schema所能做的主要是对数据形式以及操作方式进行约束，同时也包括对校验、文档、导航、访问控制的定义。

##二、 约定##
在此文档中出现的“**必须**”，“**不允许**”，“**必需**”，“**应该**”，“**不应该**”，“**推荐**”，“**可能**”，“**可选**”等与RFC 2119[RFC2119]中的定义相一致。

##三、概括##
JSON Schema基于JSON，与通常我们所使用的JSON文档相比，它所定义的媒体类型"application/json+schema"通过设定允许值，描述以及表达多资源间的关系等方式来描述JSON文档的结构。

JSON Schema由多个独立的规范组成。核心规范部分主要用于对JSON结构以及结构中元素的有效性进行描述，而Hyper规范部分则主要描述的是结构中某类可以解析为超链接的元素，以这种方式来表达多资源间的关系。终端设备因此操控对多个JSON文档。

其他JSON Schema则是用于约束某数据类型中属性值的元文档。

以描述某产品的JSON Schema为例:

    {
        "name":"Product",
        "properties":{
            "id":{
                "type":"number",
                "description":"Product identifier",
                "required":true
            },
            "name":{
                "description":"Name of the product",
                "type":"string",
                "required":true
            },
            "price":{
                "required":true,
                "type": "number",
                "minimum":0,
                "required":true
            },
            "tags":{
                "type":"array",
                "items":{
                    "type":"string"
                }
            }
        },
        "links":[
            {
                "rel":"full",
                "href":"{id}"
            },
            {
                "rel":"comments",
                "href":"comments/?id={id}"
            }
        ]
    }

上面例子中的schema描述了一个JSON文档实例中哪些属性（id，name，price）是必要的，而哪些属性(tags)是可选的。链接则表达的是JSON文档实例之间的关系。

###3.1  术语###
在本规范中，**schema**专指JSON Schema；**实例**专指schema所描述或校验的JSON值。

###3.2  考虑因素###
JSON Schema所做的不止是对JSON对象数据在结构上进行规范约束，同时也对数据如何解析、校验进行定义，在考虑扩展性的前提下，终端设备才有可能在解析JSON文档中的结构以及超链接信息时保持足够的准确度。另外，JSON文档中数据结构复杂多变的特征也要求schema的高的扩展性。

本规范与协议无关。底层协议（例如：HTTP）定义的是客户端与服务器端接口语义，做获取资源文件以及资源变动之用。而JSON schema的主要目标则不过是描述那些基于底层协议架构之上的服务所生成的复杂JSON数据的结构。

##四、Schema与实例##
JSON Schema与它相对应的实例是描述与被描述的关系。实例的媒体类型可以为"applcation/json"或者其他子类型。如何指定schmea不在本规范的论述范围。最好为实例文档中指定schema，因为这样终端设备就可以解析JSON实例以及包含的一些自描述信息并过滤到不需要的实例数据。关联实例与schema的方法有二，通过设定profile指定MIME，或者在链接头中设定describeby指向对应的schema。

例如：

    Content-Type: application/my-media-type+json;
    			  profile=http://json.com/my-hyper-schema

如果在使用某中协议传输（例如HTTP）中已经指定了头部信息，可以在链接头中指定schema：

    Link: <http://json.com/my-hyper-schema>; rel="describedby"

单个实例**可能**存在多个schema，那么实例数据**应当**通过其对应所有schema的校验。如果JSON文档是多个实例的集合，那么这个集合所包含的实例就**可能**存在多个不同schema，这样就**可能**需要在schema中设置pathStart属性来界别其应用在集合中的哪个实例。总而言之，schema的引用机制因实例文档的类型不同会存在差异。

###4.1  JSON Schema 的自描述###
JSON Schema可以实现自描述。核心规范和Hyper规范都是自描述的：

核心规范最新版：
>http://json-schema.org/schema

核心规范03版：
>http://json-schema.org/draft-03/schema

Hyper规范最新版：
>http://json-schema.org/hyper-schema

Hyper规范03版：
>http://json-schema.org/draft-03/hyper-schema
   
所有使用某一包含媒体类型定义传输协议的schema**应该**包含一个MIME参数并将其值指向自描述型的Hyper Schema或者基于Hyper Schema的扩展型schema：
    
    Content-Type: application/json;
    			  profile=http://json-schema.org/draft-03/hyper-schema

##五、核心规范定义##
JSON Schema本质上是定义多个键值对的JSON对象。JSON Schema允许递归，其结构中允许出现嵌套。

以下面的JSON Schema为例:

    {
        "description": "A person",
        "type": "object",
        "properties": {
            "name": {
                "type": "string"
            },
            "age": {
                "type": "integer",
                "maximum": 125
            }
        }
    }

JSON Schema 对象可以包含下面描述的属性，我们将其定义为schema属性(所有的属性均为可选)：

###5.1 type###
此属性对实例的原始类型或schema进行约束定义(**必须**)，其值可以为下列两种形态中的任意一种：

####简单类型####
表示原始/简单类型的字符串，允许值如下：

* string  值**必须**为字符串；
* number 值**必须**为整型（数字类型的子集）；
* boolean 值**必须**为布尔值；
* object 值**必须**为对象；
* array 值**必须**为数组；
* null 值**必须**为 null，需要注意的是，只有在使用复合(union)类型（被结合类型包含）时才允许使用；
* any 值**可能**为包含null类型的任何值；

如果属性不在列表中或是未定义，则值可以为**any**类型。出于自定义的目的，**可能**会有其他类型值，不过规范只实现了最小校验，因此任何未知类型的值的出现都是合法的。

####复合类型####
包含两个及以上简单类型定义的数组。数组中的每项均**必须**为一个简单类型定义或者schema。如果实例值符合其中任一简单类型定义或者任一schema都会被认为是合法的。

如下例，schema中定义的是实例值为string或number型任意一种定义的场景：

	{"type":["string","number"]}

###5.2 properties###
此属性用于约束定义**实例对象**中的值。当实例的**type**为**object**时，实例对象**必须**符合此对象中的属性定义。并且在此对象中，每项定义都**必须**是一个**schema**，且属性的键**必须**与实例对象中定义的属性名称保持一致。实例值**必须**通过此属性定义的校验。在此属性中的每项顺序**可能**与实例属性中的顺序不一致，这就是说此属性与顺序无关。

###5.3 patternProperties###
此属性同**properties**一样，用于约束定义实例对象的值。所不同是的，对象中的每个属性名称都是一个正则表达式（Perl形式），值则为一个schema。如果和属性名称中的正则表达式匹配到实例对象中的某属性名称。那么实例对象的属性**必须**符合对应正则表达式名称所对应schema。

###5.4 additionalProperties###
此属性为那些在对象类型中未被明确定义的属性提供一个schema。如果设定此属性，其值**必须**为一个schema或者布尔型（boolean）。如果值为false，任何附加属性将不会覆盖schema中的定义。此属性默认值为**空schema**(empty schema)，即附加属性允许为任何值。

###5.5 items###
此属性主要用于约束实例数组项的值。默认值为空**schema**，即实例数组项可以为任何值。

当此属性值为单个schema且实例值为数组时，则数组中的每项都**必须**此schema的校验。

当此属性值为一包含多个schema的数组且实例值为数组时，实例数组中每项都**必须**符合在此属性中相同下标值对应的schema。我们称之为元组(tuple)类型。在使用元组类型时，附加项是否允许出现以及addtionalItems(章节5.6)的约束关系与addtionalProperties（章节5.4）规则保持一致。

###5.6 additionalItems###
如果**items**中出现元组类型时，此属性用于定义数组实例附加项。如果值为false，则表示数组中出现的附加项是非法的。如果值为schema，则表示的是附加项所对应的schema。

###5.7 required###
此属性表示是否需要设定实例值且实例不能为undefined。默认值为false，表示此实例可选。

###5.8  dependencies###

此属性类型为object，用于表示实例对象中的属性依赖。如果在对象实例中包含此属性对象中某属性同名的情况，那么此实例就必须符合相对应属性的值（**依赖值**）。

依赖值可以是以下两种形态中任意一种：

**简单依赖** 如果依赖值为字符串，则实例对象就**必须**包含与此依赖值同名的属性。如果依赖值为一字符串数组，则实例对象就**必须**包含与依赖值数组中每一个同名的属性。

**Schema 依赖** 如果依赖值为schema，则实例对象**必须**符合此schema。

###5.9 minimum###
此属性用于定义当实例为数字类型时的实例的最小值。

###5.10 maximum###
此属性用于定义当实例为数字类型时实例的最大值。

###5.11 exclusiveMinimum###
此属性用于标示当实例为数字类型时实例值是否可以为minium中设定的最小值。默认值为false，即实例值可以大于等于最小值。

###5.12 exclusiveMaximum###
此属性用于标示当实例为数字类型时实例值是否可以为maxinum中设定的最大值。默认值为false，即实例值可以小于等于最大值。

###5.13 minItems###
此属性用于定义当实例为数组类型时所包含的最少项。
  
###5.14 maxItems###
此属性用于定义当实例为数组类型是所包含的最多项。

###5.15 uniqueItems###
此属性用于标示当实例为数组类型时数组的每项是否**必须**是唯一的。如果两个实例出现下列情况中的任意一种即被认为是相等的：

* 值都为null；
* 类型为布尔、数字或者字符串，值相等；
* 类型为数组，数组长度相等且差集为空；
* 类型为对象，含有相同个数的同名键值对；

###5.16 pattern###
此属性为一正则表达式，当实例为字符串类型时，字符串实例必须匹配此属性提供的正则表达式。此处的正则表达式**应该**符合正则表达式规范（Perl形式）。

###5.17 minLength###
此属性定义当实例为字符串类型时字符串的最小长度。

###5.18 maxLength###
此属性定义当实例值为字符串类型时字符串的最大长度。

###5.19 enum###
此属性**必须**为数组类型，用于枚举实例属性的可能值。定义此属性后，实例值必须是此数组属性中的任意一符合schema的值。枚举值与uniqueItems中定义的规则（章节5.15）保持一致。

###5.20 default###
此属性定义当实例为undefined时的默认值。

###5.21 title###
此属性为字符串类型，用于简短描述实例属性。

###5.22 description###
此属性为字符串类型，用于完整描述实例属性。

###5.23 format###
此属性用于定义实例属性值可能会出现数据类型，内容类型以及微格式。此属性**可能**是下面列表中的某一种，如果是，那么这些格式自身就附带了语义。这些格式应用场景**应当**只限于当值为原始类型（字符串，整型，数字，布尔）时。校验器**可以**(非必要)根据格式校验实例值。预设值如下：
 
* date-time 此值表示实例值**应该**符合形如YYYY-MM-DDThh:mm:ssZ的ISO 8601的UTC时间表示格式，推荐使用此格式表示日期和时间戳；
* date 此值表示实例值**应该**符合形如YYYY-MM-DD的格式。在只需要表示日期的情况下，可选择此值，其他情况则推荐使用date-time；
* time 此值表示实例值**应该**符合形如hh:mm:ss的格式。在只需要表示时间的情况下，可选择此值，其他情况则推荐使用date-time；
* utc-millisec 此值较为特殊，表示实例值**应该**为距UTC时间1970年1月1日0点0分的类型为整型或浮点型的毫秒值；
* regex 此值实例值应该为正则表达式（Perl形式）；
* color 此值表示实例值**应该**符合CSS 2.1[W3C.CR-CSS21-20070719]的色彩表现方式；
* style 此值表示实例值**应该**符合CSS 2.1[W3C.CR-CSS21-20070719]的样式规则（例如："color: red; background-color:#FFF"）；
* phone 此值表示实例值**应该**为电话号码；
* uri 此值表示实例值**应该**为URI；
* email 此值表示实例值**应该**为邮件地址；
* ip-address 此值表示实例值**应该**为ipv4地址；
* ipv6 此值表示实例值**应该**为ipv6地址；
* host-name 此值表示实例值**应该**为主机名称;

如果存在已创建的自定义格式，那么**可以**将此值设置为URI形式，且此URI指向此格式的schema。

###5.24.  divisibleBy###
此属性定义数字实例可以整除的除数。此值**不应该**为0。

###5.25 disallow###
此属性的可取的值与type属性一致，如果实例type值匹配此类型（属性值为字符串）或者匹配类型中的某一种(属性值为包含多个类型的数组)，那么此实例就是非法的。

###5.26 extends###
此属性**必须**为当前schema所继承的底层schema，任何符合当前schema的实例也**必须**符合引用的底层schema。属性**可以**是一个数组，在此场景下，实例**必须**符合数组中的每个schema。一个schema可以在另外一个schema的基础上添加附加属性，新增或修改约束条件。

从概念上讲，这里的扩展可以理解为实例的所有约束来自当前schema与所继承的schema。合并多个schema的行为尽管不是必要的，但确实是一种更优的实现。例如：

    {
        "description":"An adult",
        "properties":{"age":{"minimum": 21}},
        "extends":"person"
    }
    {
        "description":"Extended schema",
        "properties":{"deprecated":{"type": "boolean"}},
        "extends":"http://json-schema.org/draft-03/schema"
    }

###5.27 id###
此属性用于定义当前schema的URI，直白讲就是指向"self"的链接。URI**可以**是相对或绝对路径。当URI为相对路径时，如果其以包含在父级的schema中出现，则其根目录为父级schema的根目录保持一致，反之，则当前schema及其父级schema的URI都将指向当前schema的地址。在id属性没有设置的情况下，当前schema的URI将会被父级schema的URI替代。当前schema的URI通常是$ref中构造的相对路径。

###5.28 $ref###
此属性用于定义为**索引全部schema**的schema的URI。当校验器处理此属性时，当会将当前的schema替换为此属性的值后再对实例进行校验。此URI**可以**是相对或绝对路径。相对路径的根目录与当前schema所在目录一致。

###5.29 $schema###
此属性用于定义当前schema的JSON Schema。此属性被设定时，校验器**应当**使用此属性值所设定的URI指向的schema去解析Hyper Schema中的链接（章节6.1）。

校验器**可以**通过此属性的值来确定当前schema所使用JSON Schema的版本，从而提供正确的校验行为特征。因此，**推荐**所有的schema作者在其编写的schema中设定此属性以避免因JSON Schema规范版本差异出现的问题。

##六、 Hyper Schema##

   The following attributes are specified in addition to those
   attributes that already provided by the core schema with the specific
   purpose of informing user agents of relations between resources based
   on JSON data.  Just as with JSON schema attributes, all the
   attributes in hyper schemas are optional.  Therefore, an empty object
   is a valid (non-informative) schema, and essentially describes plain
   JSON (no constraints on the structures).  Addition of attributes
   provides additive information for user agents.

6.1.  links

   The value of the links property MUST be an array, where each item in
   the array is a link description object which describes the link
   relations of the instances.

6.1.1.  Link Description Object

   A link description object is used to describe link relations.  In the
   context of a schema, it defines the link relations of the instances
   of the schema, and can be parameterized by the instance values.  The
   link description format can be used on its own in regular (non-schema
   documents), and use of this format can be declared by referencing the
   normative link description schema as the the schema for the data
   structure that uses the links.  The URI of the normative link
   description schema is: http://json-schema.org/links (latest version)
   or http://json-schema.org/draft-03/links (draft-03 version).

6.1.1.1.  href

   The value of the "href" link description property indicates the
   target URI of the related resource.  The value of the instance
   property SHOULD be resolved as a URI-Reference per RFC 3986 [RFC3986]
   and MAY be a relative URI.  The base URI to be used for relative
   resolution SHOULD be the URI used to retrieve the instance object
   (not the schema) when used within a schema.  Also, when links are
   used within a schema, the URI SHOULD be parametrized by the property
   values of the instance object, if property values exist for the
   corresponding variables in the template (otherwise they MAY be
   provided from alternate sources, like user input).

   Instance property values SHOULD be substituted into the URIs where
   matching braces ('{', '}') are found surrounding zero or more
   characters, creating an expanded URI.  Instance property value
   substitutions are resolved by using the text between the braces to
   denote the property name from the instance to get the value to
   substitute.  For example, if an href value is defined:

   http://somesite.com/{id}

   Then it would be resolved by replace the value of the "id" property
   value from the instance object.  If the value of the "id" property
   was "45", the expanded URI would be:

   http://somesite.com/45

   If matching braces are found with the string "@" (no quotes) between
   the braces, then the actual instance value SHOULD be used to replace
   the braces, rather than a property value.  This should only be used
   in situations where the instance is a scalar (string, boolean, or
   number), and not for objects or arrays.

6.1.1.2.  rel

   The value of the "rel" property indicates the name of the relation to
   the target resource.  The relation to the target SHOULD be
   interpreted as specifically from the instance object that the schema
   (or sub-schema) applies to, not just the top level resource that
   contains the object within its hierarchy.  If a resource JSON
   representation contains a sub object with a property interpreted as a
   link, that sub-object holds the relation with the target.  A relation
   to target from the top level resource MUST be indicated with the
   schema describing the top level JSON representation.

   Relationship definitions SHOULD NOT be media type dependent, and
   users are encouraged to utilize existing accepted relation
   definitions, including those in existing relation registries (see RFC
   4287 [RFC4287]).  However, we define these relations here for clarity
   of normative interpretation within the context of JSON hyper schema
   defined relations:

   self  If the relation value is "self", when this property is
      encountered in the instance object, the object represents a
      resource and the instance object is treated as a full
      representation of the target resource identified by the specified
      URI.

   full  This indicates that the target of the link is the full
      representation for the instance object.  The object that contains
      this link possibly may not be the full representation.

   describedby  This indicates the target of the link is the schema for
      the instance object.  This MAY be used to specifically denote the
      schemas of objects within a JSON object hierarchy, facilitating
      polymorphic type data structures.

   root  This relation indicates that the target of the link SHOULD be
      treated as the root or the body of the representation for the
      purposes of user agent interaction or fragment resolution.  All
      other properties of the instance objects can be regarded as meta-
      data descriptions for the data.

   The following relations are applicable for schemas (the schema as the
   "from" resource in the relation):

   instances  This indicates the target resource that represents
      collection of instances of a schema.

   create  This indicates a target to use for creating new instances of
      a schema.  This link definition SHOULD be a submission link with a
      non-safe method (like POST).

   For example, if a schema is defined:

   {
     "links": [
       {
         "rel": "self"
         "href": "{id}"
       },
       {
         "rel": "up"
         "href": "{upId}"
       },
       {
         "rel": "children"
         "href": "?upId={id}"
       }
     ]
   }

   And if a collection of instance resource's JSON representation was
   retrieved:

   GET /Resource/

   [
     {
       "id": "thing",
       "upId": "parent"
     },
     {
       "id": "thing2",
       "upId": "parent"
     }
   ]

   This would indicate that for the first item in the collection, its
   own (self) URI would resolve to "/Resource/thing" and the first
   item's "up" relation SHOULD be resolved to the resource at
   "/Resource/parent".  The "children" collection would be located at
   "/Resource/?upId=thing".

6.1.1.3.  targetSchema

   This property value is a schema that defines the expected structure
   of the JSON representation of the target of the link.

6.1.1.4.  Submission Link Properties

   The following properties also apply to link definition objects, and
   provide functionality analogous to HTML forms, in providing a means
   for submitting extra (often user supplied) information to send to a
   server.

6.1.1.4.1.  method

   This attribute defines which method can be used to access the target
   resource.  In an HTTP environment, this would be "GET" or "POST"
   (other HTTP methods such as "PUT" and "DELETE" have semantics that
   are clearly implied by accessed resources, and do not need to be
   defined here).  This defaults to "GET".

6.1.1.4.2.  enctype

   If present, this property indicates a query media type format that
   the server supports for querying or posting to the collection of
   instances at the target resource.  The query can be suffixed to the
   target URI to query the collection with property-based constraints on
   the resources that SHOULD be returned from the server or used to post
   data to the resource (depending on the method).  For example, with
   the following schema:

   {
    "links":[
      {
        "enctype":"application/x-www-form-urlencoded",
        "method":"GET",
        "href":"/Product/",
        "properties":{
           "name":{"description":"name of the product"}
        }
      }
    ]
   }

   This indicates that the client can query the server for instances
   that have a specific name:

   /Product/?name=Slinky

   If no enctype or method is specified, only the single URI specified
   by the href property is defined.  If the method is POST,
   "application/json" is the default media type.

6.1.1.4.3.  schema

   This attribute contains a schema which defines the acceptable
   structure of the submitted request (for a GET request, this schema
   would define the properties for the query string and for a POST
   request, this would define the body).

6.2.  fragmentResolution

   This property indicates the fragment resolution protocol to use for
   resolving fragment identifiers in URIs within the instance
   representations.  This applies to the instance object URIs and all
   children of the instance object's URIs.  The default fragment
   resolution protocol is "slash-delimited", which is defined below.
   Other fragment resolution protocols MAY be used, but are not defined
   in this document.

   The fragment identifier is based on RFC 2396, Sec 5 [RFC2396], and
   defines the mechanism for resolving references to entities within a
   document.

6.2.1.  slash-delimited fragment resolution

   With the slash-delimited fragment resolution protocol, the fragment
   identifier is interpreted as a series of property reference tokens
   that start with and are delimited by the "/" character (\x2F).  Each
   property reference token is a series of unreserved or escaped URI
   characters.  Each property reference token SHOULD be interpreted,
   starting from the beginning of the fragment identifier, as a path
   reference in the target JSON structure.  The final target value of
   the fragment can be determined by starting with the root of the JSON
   structure from the representation of the resource identified by the
   pre-fragment URI.  If the target is a JSON object, then the new
   target is the value of the property with the name identified by the
   next property reference token in the fragment.  If the target is a
   JSON array, then the target is determined by finding the item in
   array the array with the index defined by the next property reference
   token (which MUST be a number).  The target is successively updated
   for each property reference token, until the entire fragment has been
   traversed.

   Property names SHOULD be URI-encoded.  In particular, any "/" in a
   property name MUST be encoded to avoid being interpreted as a
   property delimiter.

   For example, for the following JSON representation:

   {
     "foo":{
       "anArray":[
         {"prop":44}
       ],
       "another prop":{
         "baz":"A string"
       }
     }
   }

   The following fragment identifiers would be resolved:

   fragment identifier      resolution
   -------------------      ----------
   #                        self, the root of the resource itself
   #/foo                    the object referred to by the foo property
   #/foo/another%20prop     the object referred to by the "another prop"
                            property of the object referred to by the
                            "foo" property
   #/foo/another%20prop/baz the string referred to by the value of "baz"
                            property of the "another prop" property of
                            the object referred to by the "foo" property
   #/foo/anArray/0          the first object in the "anArray" array

6.2.2.  dot-delimited fragment resolution

   The dot-delimited fragment resolution protocol is the same as slash-
   delimited fragment resolution protocol except that the "." character
   (\x2E) is used as the delimiter between property names (instead of
   "/") and the path does not need to start with a ".".  For example,
   #.foo and #foo are a valid fragment identifiers for referencing the
   value of the foo propery.

6.3.  readonly

   This attribute indicates that the instance property SHOULD NOT be
   changed.  Attempts by a user agent to modify the value of this
   property are expected to be rejected by a server.

6.4.  contentEncoding

   If the instance property value is a string, this attribute defines
   that the string SHOULD be interpreted as binary data and decoded
   using the encoding named by this schema property.  RFC 2045, Sec 6.1
   [RFC2045] lists the possible values for this property.

6.5.  pathStart

   This attribute is a URI that defines what the instance's URI MUST
   start with in order to validate.  The value of the "pathStart"
   attribute MUST be resolved as per RFC 3986, Sec 5 [RFC3986], and is
   relative to the instance's URI.

   When multiple schemas have been referenced for an instance, the user
   agent can determine if this schema is applicable for a particular
   instance by determining if the URI of the instance begins with the
   the value of the "pathStart" attribute.  If the URI of the instance
   does not start with this URI, or if another schema specifies a
   starting URI that is longer and also matches the instance, this
   schema SHOULD NOT be applied to the instance.  Any schema that does
   not have a pathStart attribute SHOULD be considered applicable to all
   the instances for which it is referenced.

6.6.  mediaType

   This attribute defines the media type of the instance representations
   that this schema is defining.

7.  Security Considerations

   This specification is a sub-type of the JSON format, and consequently
   the security considerations are generally the same as RFC 4627
   [RFC4627].  However, an additional issue is that when link relation
   of "self" is used to denote a full representation of an object, the
   user agent SHOULD NOT consider the representation to be the
   authoritative representation of the resource denoted by the target
   URI if the target URI is not equivalent to or a sub-path of the the
   URI used to request the resource representation which contains the
   target URI with the "self" link.  For example, if a hyper schema was
   defined:
   {
     "links":[
           {
             "rel":"self",
             "href":"{id}"
           }
     ]
   }

   And a resource was requested from somesite.com:

   GET /foo/

   With a response of:

Content-Type: application/json; profile=/schema-for-this-data
[
  {"id":"bar", "name":"This representation can be safely treated \
        as authoritative "},
  {"id":"/baz", "name":"This representation should not be treated as \
        authoritative the user agent should make request the resource\
        from "/baz" to ensure it has the authoritative representation"},
  {"id":"http://othersite.com/something", "name":"This representation\
        should also not be treated as authoritative and the target\
        resource representation should be retrieved for the\
        authoritative representation"}
]

8.  IANA Considerations

   The proposed MIME media type for JSON Schema is "application/
   schema+json".

   Type name: application

   Subtype name: schema+json

   Required parameters: profile

   The value of the profile parameter SHOULD be a URI (relative or
   absolute) that refers to the schema used to define the structure of
   this structure (the meta-schema).  Normally the value would be
   http://json-schema.org/draft-03/hyper-schema, but it is allowable to
   use other schemas that extend the hyper schema's meta- schema.

   Optional parameters: pretty

   The value of the pretty parameter MAY be true or false to indicate if
   additional whitespace has been included to make the JSON
   representation easier to read.

8.1.  Registry of Link Relations

   This registry is maintained by IANA per RFC 4287 [RFC4287] and this
   specification adds four values: "full", "create", "instances",
   "root".  New assignments are subject to IESG Approval, as outlined in
   RFC 5226 [RFC5226].  Requests should be made by email to IANA, which
   will then forward the request to the IESG, requesting approval.

9.  References

9.1.  Normative References

   [RFC2045]                          Freed, N. and N. Borenstein,
                                      "Multipurpose Internet Mail
                                      Extensions (MIME) Part One: Format
                                      of Internet Message Bodies",
                                      RFC 2045, November 1996.

   [RFC2119]                          Bradner, S., "Key words for use in
                                      RFCs to Indicate Requirement
                                      Levels", BCP 14, RFC 2119,
                                      March 1997.

   [RFC2396]                          Berners-Lee, T., Fielding, R., and
                                      L. Masinter, "Uniform Resource
                                      Identifiers (URI): Generic
                                      Syntax", RFC 2396, August 1998.

   [RFC3339]                          Klyne, G., Ed. and C. Newman,
                                      "Date and Time on the Internet:
                                      Timestamps", RFC 3339, July 2002.

   [RFC3986]                          Berners-Lee, T., Fielding, R., and
                                      L. Masinter, "Uniform Resource
                                      Identifier (URI): Generic Syntax",
                                      STD 66, RFC 3986, January 2005.

   [RFC4287]                          Nottingham, M., Ed. and R. Sayre,
                                      Ed., "The Atom Syndication
                                      Format", RFC 4287, December 2005.

9.2.  Informative References

   [RFC2616]                          Fielding, R., Gettys, J., Mogul,
                                      J., Frystyk, H., Masinter, L.,



Zyp & Court               Expires May 26, 2011                 [Page 24]

Internet-Draft           JSON Schema Media Type            November 2010


                                      Leach, P., and T. Berners-Lee,
                                      "Hypertext Transfer Protocol --
                                      HTTP/1.1", RFC 2616, June 1999.

   [RFC4627]                          Crockford, D., "The application/
                                      json Media Type for JavaScript
                                      Object Notation (JSON)", RFC 4627,
                                      July 2006.

   [RFC5226]                          Narten, T. and H. Alvestrand,
                                      "Guidelines for Writing an IANA
                                      Considerations Section in RFCs",
                                      BCP 26, RFC 5226, May 2008.

   [I-D.hammer-discovery]             Hammer-Lahav, E., "LRDD: Link-
                                      based Resource Descriptor
                                      Discovery",
                                      draft-hammer-discovery-06 (work in
                                      progress), May 2010.

   [I-D.gregorio-uritemplate]         Gregorio, J., Fielding, R.,
                                      Hadley, M., and M. Nottingham,
                                      "URI Template",
                                      draft-gregorio-uritemplate-04
                                      (work in progress), March 2010.

   [I-D.nottingham-http-link-header]  Nottingham, M., "Web Linking", dra
                                      ft-nottingham-http-link-header-10
                                      (work in progress), May 2010.

   [W3C.REC-html401-19991224]         Raggett, D., Hors, A., and I.
                                      Jacobs, "HTML 4.01 Specification",
                                      World Wide Web Consortium Recommen
                                      dation REC-html401-19991224,
                                      December 1999, <http://www.w3.org/
                                      TR/1999/REC-html401-19991224>.

   [W3C.CR-CSS21-20070719]            Hickson, I., Lie, H., Celik, T.,
                                      and B. Bos, "Cascading Style
                                      Sheets Level 2 Revision 1 (CSS
                                      2.1) Specification", World Wide
                                      Web Consortium CR CR-CSS21-
                                      20070719, July 2007, <http://
                                      www.w3.org/TR/2007/
                                      CR-CSS21-20070719>.






Zyp & Court               Expires May 26, 2011                 [Page 25]

Internet-Draft           JSON Schema Media Type            November 2010


Appendix A.  Change Log

   draft-03

      *  Added example and verbiage to "extends" attribute.

      *  Defined slash-delimited to use a leading slash.

      *  Made "root" a relation instead of an attribute.

      *  Removed address values, and MIME media type from format to
         reduce confusion (mediaType already exists, so it can be used
         for MIME types).

      *  Added more explanation of nullability.

      *  Removed "alternate" attribute.

      *  Upper cased many normative usages of must, may, and should.

      *  Replaced the link submission "properties" attribute to "schema"
         attribute.

      *  Replaced "optional" attribute with "required" attribute.

      *  Replaced "maximumCanEqual" attribute with "exclusiveMaximum"
         attribute.

      *  Replaced "minimumCanEqual" attribute with "exclusiveMinimum"
         attribute.

      *  Replaced "requires" attribute with "dependencies" attribute.

      *  Moved "contentEncoding" attribute to hyper schema.

      *  Added "additionalItems" attribute.

      *  Added "id" attribute.

      *  Switched self-referencing variable substitution from "-this" to
         "@" to align with reserved characters in URI template.

      *  Added "patternProperties" attribute.

      *  Schema URIs are now namespace versioned.

      *  Added "$ref" and "$schema" attributes.




Zyp & Court               Expires May 26, 2011                 [Page 26]

Internet-Draft           JSON Schema Media Type            November 2010


   draft-02

      *  Replaced "maxDecimal" attribute with "divisibleBy" attribute.

      *  Added slash-delimited fragment resolution protocol and made it
         the default.

      *  Added language about using links outside of schemas by
         referencing its normative URI.

      *  Added "uniqueItems" attribute.

      *  Added "targetSchema" attribute to link description object.

   draft-01

      *  Fixed category and updates from template.

   draft-00

      *  Initial draft.

Appendix B.  Open Issues

      Should we give a preference to MIME headers over Link headers (or
      only use one)?

      Should "root" be a MIME parameter?

      Should "format" be renamed to "mediaType" or "contentType" to
      reflect the usage MIME media types that are allowed?

      How should dates be handled?

Authors' Addresses

   Kris Zyp (editor)
   SitePen (USA)
   530 Lytton Avenue
   Palo Alto, CA 94301
   USA

   Phone: +1 650 968 8787
   EMail: kris@sitepen.com







Zyp & Court               Expires May 26, 2011                 [Page 27]

Internet-Draft           JSON Schema Media Type            November 2010


   Gary Court
   Calgary, AB
   Canada

   EMail: gary.court@gmail.com














































Zyp & Court               Expires May 26, 2011                 [Page 28]

