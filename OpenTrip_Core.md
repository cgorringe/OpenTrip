# OpenTrip Core

Draft \#1: February 17, 2009


## Notice

This is a draft document for public review. The contents of this document may change at any time and is not ready for use in implementations.


## Introduction

OpenTrip is a group of specifications for the interchange of travel information among carpool, rideshare, and related services. It includes the following:

<dl>
  <dt>OpenTrip Core</dt>
  <dd>Primary OpenTrip Atom feed.</dd>
  <dt>OpenTrip Ping</dt>
  <dd>Method of pinging feed consumers of updated feeds.</dd>
  <dt>OpenTrip Search</dt>
  <dd>Uses <a href="http://www.opensearch.org">OpenSearch</a> to return an OpenTrip Core feed.</dd>
  <dt>OpenTrip Dynamic</dt>
  <dd>Extensions for supporting Dynamic Carpooling applications.</dd>
</dl>

This document describes only the ***OpenTrip Core*** specification.


### Notational Conventions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC2119](http://tools.ietf.org/html/rfc2119).


### Namespace

The XML Namespaces URI for the XML data formats described in this specification have not been finalized. A temporary namespace which may be used is: `http://opentrip.info/-/opentrip/0.1/`


### Glossary

<dl>
  <dt>Date</dt>
  <dd>A Date in OpenTrip is specified by <a href="http://tools.ietf.org/html/rfc3339">RFC3339</a>, the same used in the Atom Syndication Format. See <a href="http://tools.ietf.org/html/rfc4287#section-3.3">RFC4287 section 3.3</a>.
  </dd>
</dl>


## OpenTrip Core Feed Format

### Overview

(TODO)


### Basic Atom Feed Structure

An OpenTrip feed data structure uses and extends the Atom Syndication Format as defined in [RFC4287](http://tools.ietf.org/html/rfc4287). It also uses the [GeoRSS Simple](http://georss.org/simple) extension. OpenTrip defines additional tags using custom extensions.

This is the basic framework of an Atom feed:

```xml
<?xml version="1.0" encoding="utf-8"?>
<feed xmlns="http://www.w3.org/2005/Atom">
  feed elements
  <entry>
     entry elements
  </entry>
  <entry>
     entry elements
  </entry>
  ...
</feed>
```

An "entry" in a OpenTrip feed represents a single *Trip*. A feed is composed of a collection of *Trips*.


### Example OpenTrip Feed

```xml
<?xml version="1.0" encoding="utf-8"?>
<feed xmlns="http://www.w3.org/2005/Atom" 
      xmlns:g="http://www.georss.org/georss"
      xmlns:t="http://opentrip.info/-/opentrip/0.1/">
  <title>Example Feed</title>
  <link rel="self" href="http://example.com/feeds/foobar.xml"/>
  <id>urn:guid:example.com:ABCDEFG</id>
  <updated>2009-01-01T01:23:45Z</updated>
  <author><name></name></author>
  <entry>
    <title>I need a ride</title>
    <link href="http://example.com/postings/123456789.html"/>
    <id>urn:guid:example.com:123456789</id>
    <published>2009-01-01T01:23:45Z</published>
    <updated>2009-01-01T01:23:45Z</updated>
    <t:expires>2009-09-01T01:23:45Z</t:expires>
    <content>I'm looking for a ride to work and back home during the week.</content>
    <t:location label="Home">
      <t:town>Oakland</t:town>
      <t:region>CA</t:region>
      <t:country>US</t:country>
      <g:point>37.774311 -122.214746</g:point>
      <t:leaves recurs="weekly" days="MTWHF" offset="30">2009-04-01T08:00:00Z</t:leaves>
      <t:returns recurs="weekly" days="MTWHF" offset="30">2009-04-01T18:00:00Z</t:returns>
    </t:location>
    <t:location label="Work">
      <t:town>San Francisco</t:town>
      <t:region>CA</t:region>
      <t:country>US</t:country>
      <g:point>37.779806 -122.419925</g:point>
    </t:location>
    <author>
      <name>John Doe</name>
      <email>john@example.com</email>
      <uri>http://example.com/profile123.html</uri>
      <t:phone label="mobile">510-555-1234</t:phone>
      <t:age>25</t:age><t:gender>male</t:gender>
    </author>
    <t:prefs>
      <t:drive/><t:nonsmoking/>
    </t:prefs>
    <t:mode kind="auto">
      <t:cost kind="USD">2.00</t:cost>
      <t:capacity>2</t:capacity>
      <t:vacancy>1</t:vacancy>
      <t:make>Tesla</t:make>
      <t:model>Roadster</t:model>
      <t:year>2009</t:year>
      <t:color>Red</t:color>
      <t:lic>ABCD123</t:lic>
    </t:mode>
  </entry>
</feed>
```


### Feed Elements

All Atom elements may be used in a OpenTrip feed, since it is also an Atom feed. See [RFC4287](http://tools.ietf.org/html/rfc4287) for the full Atom specification, [section 4.1.1](http://tools.ietf.org/html/rfc4287#section-4.1.1) for "feed" child elements.


#### Example

```xml
<feed xmlns="http://www.w3.org/2005/Atom" 
      xmlns:g="http://www.georss.org/georss"
      xmlns:t="http://opentrip.info/-/opentrip/0.1/">
  <title>Example Feed</title>
  <link rel="self" href="http://example.com/feeds/foobar.xml"/>
  <id>urn:guid:example.com:ABCDEFG</id>
  <updated>2009-01-01T01:23:45Z</updated>
  <author><name></name></author>
  <entry> ... </entry>
  <entry> ... </entry>
  ...
</feed>
```


#### The "atom:title" Element
  
`Atom:feed` elements MUST contain exactly one `atom:title` element. The `title` element at feed level is not used with OpenTrip, but is required under Atom.


#### The "atom:link" Element
  
`Atom:feed` elements SHOULD contain one `atom:link` element with a `rel` attribute value of "self". The value of the `href` attribute is a URI that points to the preferred location of retrieving this OpenTrip feed.


#### The "atom:id" Element
  
`Atom:feed` elements MUST contain exactly one `atom:id` element, which MUST be an IRI as defined by [RFC3987](http://tools.ietf.org/html/rfc3987). See [RFC4287 section 4.2.6](http://tools.ietf.org/html/rfc4287#section-4.2.6). The `id` element at feed level is not used with OpenTrip, but is required under Atom.


#### The "atom:updated" Element
  
`Atom:feed` elements MUST contain exactly one `atom:updated` element. The `atom:updated` element is a [Date](#glossary) construct indicating the most recent instant in time when a feed was last modified.


#### The "atom:author" Element
  
`Atom:feed` elements MUST contain one or more `atom:author` elements, unless all of the `atom:feed` element's child `atom:entry` elements contain at least one `atom:author` element. The `author` element at feed level is not used with OpenTrip. The following empty author element may be used:

```xml
<author><name></name></author>
```

### Entry Elements

The `atom:entry` element represents an individual entry, acting as a container for metadata and data associated with the entry. This element can appear as a child of the `atom:feed` element, or it can appear as the document (i.e., top-level) element of a stand-alone Atom Entry Document. See [RFC4287 section 4.1.2](http://tools.ietf.org/html/rfc4287#section-4.1.2). An "entry" in a OpenTrip feed represents a single *Trip*.


#### Example

```xml
<entry>
  <title>I need a ride</title>
  <link href="http://example.com/postings/123456789.html"/>
  <id>urn:guid:example.com:123456789</id>
  <published>2009-01-01T01:23:45Z</published>
  <updated>2009-01-01T01:23:45Z</updated>
  <t:expires>2009-09-01T01:23:45Z</t:expires>
  <content>I'm looking for a ride to work and back home during the week.</content>
  <t:location> ... </t:location>
  <author> ... </author>
  <t:prefs> ... </t:prefs>
  <t:mode> ... </t:mode>
</entry>
```


#### The "atom:title" Element
  
`Atom:entry` elements MUST contain exactly one `atom:title` element. The `title` element at entry level is used in OpenTrip to give a title to the trip that the entry represents.


#### The "atom:link" Element
  
OpenTrip `atom:entry` elements MUST contain one `atom:link` element with either a `rel` attribute not present, or one present with a value of "alternate". The value of the `href` attribute is a URI that points to detailed Trip information on the source website.


#### The "atom:id" Element
  
`Atom:entry` elements MUST contain exactly one `atom:id` element, which MUST be an IRI as defined by [RFC3987](http://tools.ietf.org/html/rfc3987). See [RFC4287 section 4.2.6](http://tools.ietf.org/html/rfc4287#section-4.2.6). Additionally, the `id` element at entry level must conform to the following rules:

1.  The id MUST begin with "urn:guid:"

2.  The next segment MUST be a domain name that uniquely identifies the website providing the feed data. The formation of the domain name MUST follow these rules:
    -   The chosen domain name SHOULD NOT include subdomains such as "www", but MAY include subdomains which are part of the website's address. For example, "example.com" or "foobar.example.com" MAY be used, while "www.example.com" SHOULD NOT be used.
    -   If the data feed is hosted on a website domain which is independent and not representative of the host of the feed, then the feed owner MAY use a domain name that they own, even though the feed is hosted elsewhere.
    -   If the feed owner does not own a suitable domain name, one can be created by choosing a unique name and prepending it to the domain "opentrip.info". (e.g. "mycarpoolsite.opentrip.info"). When choosing this option, the feed owner SHOULD register their chosen name with the owner of the `opentrip.info` domain.
    -   For testing purposes only, a name such as "test1.example.com" may be chosen, where "example.com" is used literally, since this domain name is reserved for testing purposes. Any of the domain names specified in [RFC2606](http://tools.ietf.org/html/rfc2606) may be used for testing purposes.

3.  Following the domain name is a colon and a unique Trip ID. The Trip ID SHALL NOT include characters other than letters (A-Za-z), numbers (0-9), dots (.), hyphens (-) and underscores (\_).

4.  The full length of the id, including the "urn:guid:" prefix, MUST NOT exceed 64 characters.

Example of a valid OpenTrip ID:

```xml
<id>urn:guid:example.com:ed52bw8k345</id>
```


#### The "atom:published" Element
  
`Atom:entry` elements MUST NOT contain more than one `atom:published` element. The `atom:published` element is a [Date](#glossary) construct indicating when the entry was first created, i.e. when a Trip was originally posted.


#### The "atom:updated" Element
  
`Atom:entry` elements MUST contain exactly one `atom:updated` element. The `atom:updated` element is a [Date](#glossary) construct indicating the most recent instant in time when a entry was last modified.


#### The "opentrip:expires" Element
  
OpenTrip `atom:entry` elements MUST contain exactly one `opentrip:expires` element. The `opentrip:expires` element is a [Date](#glossary) construct indicating when a Trip should expire. If the Date provided precedes the current date and time, the entry expires immediately.


#### The "atom:content" Element
  
`Atom:entry` elements MUST NOT contain more than one `atom:content` element. This element either contains or links to the content of the entry, which MAY convey additional trip information provided by the user. If the content is provided, the `type` attribute SHOULD either not be present, or have the value of "text". `Atom:content` MAY have a "src" attribute as described in Atom [RFC4287 section 4.1.3.2](http://tools.ietf.org/html/rfc4287#section-4.1.3.2).


#### The "opentrip:location" Element
  
`Atom:entry` elements MUST contain at least one `atom:location` element, and SHOULD contain at least two. This element is a container described in the section [Location Constructs](#location-constructs).


#### The "atom:author" Element
  
`Atom:entry` elements MUST NOT contain more than one `atom:author` element. This element is a container described in the section [Person Constructs](#person-constructs).


#### The "opentrip:prefs" Element
  
`Atom:entry` elements MUST NOT contain more than one `atom:prefs` element. This element is a container described in the section [Preference Constructs](#preference-constructs).


#### The "opentrip:mode" Element
  
`Atom:entry` elements MAY contain one or more `atom:mode` elements. This element is a container described in the section [Mode Constructs](#mode-constructs).


### Location Constructs

The `opentrip:location` element is a container that represents a single location and either a single or multiple date-times. A location may be represented by a *Named Location*, a *Coordinate*, or both. Date-times may either represent a single trip, a leaving and returning trip (i.e. round-trip), or a more complex recurring trip.

Multiple Location constructs are distinguished by being specified as either an *origin*, *destination*, or *waypoint*. A location's designation is typically implied by the ordering of the location construct in relation to other location constructs within an entry. The *first* location is the "origin", the *last* location is the "destination", and any in between (when present) are "waypoints". The ordering of the location constructs thus determines the order of the *leaving* direction, while the reverse ordering determines the *returning* direction.

A location's designation may be overridden by specifying the `point` attribute. This is only necessary when, for example, there is only a single location construct present and therefore no way to determine if it represents an origin or destination.


#### Example

```xml
<t:location point="orig" label="Home">
  <t:town>Oakland</t:town>
  <t:region>CA</t:region>
  <t:country>US</t:country>
  <g:point>37.774311 -122.214746</g:point>
  <t:leaves recurs="weekly" days="MTWHF" offset="30">2009-04-01T08:00:00Z</t:leaves>
  <t:returns recurs="weekly" days="MTWHF" offset="30">2009-04-01T18:00:00Z</t:returns>
</t:location>
```


#### Attributes

The following attributes of `opentrip:location` are OPTIONAL:

<dl>
  <dt>point</dt>
  <dd>May be one of `orig` (origin), `dest` (destination), or `wayp` (waypoint).</dd>
  <dt>label</dt>
  <dd>A human-readable Text construct giving a name to the location.</dd>
</dl>


#### Named Location Elements

A location may be determined by providing a place name categorized by levels of specificity. Location constructs MUST NOT contain more than one of each of the following elements, and their ordering is unimportant.

The following elements use the `opentrip` namespace:

<dl>
  <dt>country</dt>
  <dd>2-letter country code defined in <a href="http://en.wikipedia.org/wiki/ISO_3166-1">ISO 3166-1 alpha-2</a>.</dd>
  <dt>region</dt>
  <dd>State, province, or prefecture. Use abbreviated form used in the second part of <a href="http://en.wikipedia.org/wiki/ISO_3166-2">ISO 3166-2</a> codes. (i.e. Use 2-letter state abbreviations for states in the United States.)</dd>
  <dt>subregion</dt>
  <dd>Full name of county or municipality.</dd>
  <dt>town</dt>
  <dd>Full name of city or village.</dd>
  <dt>postcode</dt>
  <dd>Zip code or postal code.</dd>
  <dt>street</dt>
  <dd>Street name.</dd>
  <dt>intersection</dt>
  <dd>Closest intersecting street name.</dd>
  <dt>address</dt>
  <dd>Complete address, which may include the street, town, region, and country, or what would otherwise be included in a full address. (e.g. "123 Main St., San Francisco, CA, US")</dd>
</dl>


#### Coordinate Elements

Coordinates are provided using the [GeoRSS-Simple](http://georss.org/simple) extensions. Including a `georss:point` element in every location construct is highly recommended. Location constructs MUST NOT contain more than one of each of the following elements, and their ordering is unimportant.

<dl>
  <dt>georss:point</dt>
  <dd>Contains a single latitude-longitude pair, separated by whitespace. The `georss:point` element is RECOMMENDED.</dd>
  <dt>georss:radius</dt>
  <dd>Indicates the size in meters of an approximate radius of the covered area around the point provided. The `georss:radius` element is OPTIONAL.</dd>
  <dt>georss:box</dt>
  <dd>A rectangular region, used to define the area covered. A box contains two space-separated latitude-longitude pairs, with each pair separated by whitespace. The first pair is the SW corner, the second is the NE corner. The `georss:box` element is OPTIONAL.</dd>
</dl>


#### Date-Time Elements

The elements `opentrip:leaves` and `opentrip:returns` have identical format, and are referred to collectively as the *date-time elements*. The `leaves` element is used for one-way trips and for the leaving direction of a round trip. The `returns` element is used for the returning direction of a round trip. One can determine if a trip is a one-way or round trip by the absence or presence of the `returns` element.

In the absence of the `recurs` attribute, a date-time element's value is a Date construct which represents the ideal date and time that a person wishes to be present for departure or arrival at the location provided. In this case it represents a one-time trip.

In the presence of the `recurs` attribute, a date-time element's value is a Date construct representing only the first of multiple recurring trips. Trips will continue to recur until the date-time provided in the `opentrip:expires` element passes.

A Location construct MAY contain multiple date-time elements.


##### Attributes

<dl>
  <dt>offset</dt>
  <dd>The date-time value is offset by plus or minus the offset attribute value in minutes. This value allows for the use of a time range rather than an exact date-time.</dd>
  <dt>recurs</dt>
  <dd>Used to specify a recurring trip. Valid values are `weekly`, `biweekly`, and `monthly`. A weekly recurring trip is determined by taking the date-time provided as the first date, and repeatedly adding 7 days to the date portion, while keeping the time portion, until `opentrip:expires` is reached. For a biweekly recurring trip add 14 days instead. A monthly recurring trip is similarly determined by repeatedly adding 1 month to the date portion.</dd>
  <dt>days</dt>
  <dd>Additional days of the week may be included in a weekly or biweekly recurring trip by specifying the `days` attribute. The `days` attribute is not defined for monthly recurring trips. Use the following single-letter codes to represent the days of the week: M = Monday, T = Tuesday, W = Wednesday, H = Thursday, F = Friday, S = Saturday, U = Sunday.</dd>
</dl>


##### Examples

*   One-time Trip, *departing* between 8am and 9am on Wed April 1, 2009. Place this element within the *origin* location. To specify an *arrival* time instead, place this within the *destination* location:

    ```xml
<t:leaves offset="30">2009-04-01T08:30:00Z</t:leaves>
```

*   One-time Round-trip, departing Wed April 1, 2009 at 10am and returning 2 days later at 7pm:

    ```xml
<t:leaves>2009-04-01T10:00:00Z</t:leaves>
<t:returns>2009-04-03T19:00:00Z</t:returns>
```

*   Weekly Recurring One-way trip, leaving 6pm every Wednesday beginning Wed April 1, 2009:

    ```xml
<t:leaves recurs="weekly">2009-04-01T18:00:00Z</t:leaves>
```

*   Weekly Recurring Round-trip, leaving 8-8:30am Monday through Friday and returning at 6:30pm, beginning Wed April 1, 2009:

    ```xml
<t:leaves recurs="weekly" days="MTWHF" offset="15">2009-04-01T08:15:00Z</t:leaves>
<t:returns recurs="weekly" days="MTWHF">2009-04-01T18:30:00Z</t:returns>
```

*   Weekly Recurring Round-trip, leaving 10am on Mon / Wed / Fri, 12 noon Tues / Thur, and returning 6pm every weekday, beginning Wed April 1, 2009:

    ```xml
<t:leaves recurs="weekly" days="MWF">2009-04-01T10:00:00Z</t:leaves>
<t:leaves recurs="weekly" days="TH">2009-04-02T12:00:00Z</t:leaves>
<t:returns recurs="weekly" days="MTWHF">2009-04-01T18:00:00Z</t:returns>
```

*   Monthly Recurring Round-trip, on the 2nd day of every month, leaving 6am, returning 5pm, beginning Thurs April 2, 2009:

    ```xml
<t:leaves recurs="monthly">2009-04-02T06:00:00Z</t:leaves>
<t:returns recurs="monthly">2009-04-02T17:00:00Z</t:returns>
```


### Person Constructs

An `atom:author` element is a Person construct as described in [RFC4287 section 3.2](http://tools.ietf.org/html/rfc4287#section-3.2). It is used to provide information about the person who created the entry, and is OPTIONAL. Additional extensions are defined by OpenTrip.


#### Example

```xml
<author>
  <name>John Doe</name>
  <email>john@example.com</email>
  <uri>http://example.com/profile123.html</uri>
  <t:uri>http://example.org/alternate_profile123.html</t:uri>
  <t:phone label="mobile">510-555-1234</t:phone>
  <t:age>25</t:age>
  <t:gender>male</t:gender>
</author>
```


#### Identity Elements

Identity elements encode identity information for this person construct.

<dl>
  <dt>atom:name</dt>
  <dd>The `atom:name` element's content conveys a human-readable name for the person. Person constructs MUST contain exactly one `atom:name` element.</dd>
  <dt>opentrip:alias</dt>
  <dd>The `opentrip:alias` element's content conveys an alternate name, handle, or alias associated with the person, preferably for displaying in lieu of the person's real name. Person constructs MUST NOT contain more than one `opentrip:alias`.</dd>
  <dt>opentrip:userid</dt>
  <dd>The `opentrip:userid` element's content conveys a user's id code, typically used internally within the feed source's database. Its content SHALL NOT include characters other than numbers (0-9) or letters (A-Za-z). Person constructs MUST NOT contain more than one `opentrip:userid`.</dd>
</dl>


#### Contact Elements

Contact elements encode contact information for this person construct.

<dl>
  <dt>atom:email</dt>
  <dd>The `atom:email` element's content conveys an e-mail address associated with the person. Person constructs MAY contain an `atom:email` element, but MUST NOT contain more than one. Its content MUST conform to the "addr-spec" production in <a href="http://tools.ietf.org/html/rfc2822#section-3.4.1">RFC2822 section 3.4.1</a>.</dd>
  <dt>atom:uri</dt>
  <dd>The `atom:uri` element's content conveys an IRI associated with the person. Person constructs MAY contain an `atom:uri` element, but MUST NOT contain more than one. The content of `atom:uri` in a Person construct MUST be an IRI reference <a href="http://tools.ietf.org/html/rfc3987">RFC3987</a>.</dd>
  <dt>opentrip:uri</dt>
  <dd>The `opentrip:uri` element's content conveys an IRI associated with the person. Person constructs MAY contain any number of `opentrip:uri` elements. This element is used in the same way as `atom:uri`. Its purpose is to extend `atom:uri` by allowing additional IRIs per Person construct, since the Atom specification only allows at most one `atom:uri` element. `Atom:uri` SHOULD be used first before adding additional URIs using `opentrip:uri`.</dd>
  <dt>opentrip:phone</dt>
  <dd>The `opentrip:phone` element's content conveys a phone number associated with the person. Person constructs MAY contain any number of `opentrip:phone` elements. Its content SHALL NOT include characters other than numbers (0-9), dots (.), hyphens (-), plus signs (+), parentheses, or spaces. This element MAY have an optional `label` attribute. Its value is a Text construct intended for human readability. Recommended values include `mobile`, `home`, and `work`, but may also include additional values not defined here.  
  </dd>
</dl>


#### Trait Elements

Trait elements encode characteristics of the person defined in this person construct. The following elements use the `opentrip` namespace. Person constructs MAY contain any of the following elements in any order, but MUST NOT contain more than one of each.

<dl>
  <dt>age</dt>
  <dd>The `opentrip:age` element's content conveys the age of a person as a single integer, or a range of two integers separated by a hyphen.</dd>
  <dt>gender</dt>
  <dd>The `opentrip:gender` element's content conveys a person's gender, which shall contain the value `male` or `female`.</dd>
  <dt>smoker</dt>
  <dd>The `opentrip:smoker` element's presence conveys that the person wishes to smoke. Specify as an empty entry. (e.g. &lt;smoker/&gt;)</dd>
  <dt>blind</dt>
  <dd>The `opentrip:blind` element's presence conveys that the person is seeing impaired and may need special accommodation. Specify as an empty entry. (e.g. &lt;blind/&gt;)</dd>
  <dt>deaf</dt>
  <dd>The `opentrip:deaf` element's presence conveys that the person is hearing impaired and may need special accommodation. Specify as an empty entry. (e.g. &lt;deaf/&gt;)</dd>
  <dt>dog</dt>
  <dd>The `opentrip:dog` element's presence conveys that the person wishes to bring along a service animal or guide dog. Specify as an empty entry. (e.g. &lt;dog/&gt;)</dd>
</dl>


### Preference Constructs

The optional `opentrip:prefs` element is a container that represents trip preferences.

#### Example

```xml
<t:prefs>
  <t:drive/><t:ride/>
  <t:age>18-30</t:age>
  <t:gender>female</t:gender>
  <t:nonsmoking/>
</t:prefs>
```


#### Elements

The following elements use the `opentrip` namespace. Preference constructs MAY contain any of the following elements in any order, but MUST NOT contain more than one of each.

<dl>
  <dt>drive</dt>
  <dd>The `opentrip:drive` element's presence conveys that the person could drive. Specify as an empty entry. (e.g. &lt;drive/&gt;)</dd>
  <dt>ride</dt>
  <dd>The `opentrip:ride` element's presence conveys that the person seeks a ride as a passenger. Specify as an empty entry. (e.g. &lt;ride/&gt;)</dd>
  <dt>age</dt>
  <dd>The `opentrip:age` element's content conveys the preferred age range that the person wishes to travel with. Its content MUST be either a single integer, or a range of two integers separated by a hyphen.</dd>
  <dt>gender</dt>
  <dd>The `opentrip:gender` element's content conveys a preferred gender that the person wishes to travel with. Its content MUST contain the value `male` or `female`.</dd>
  <dt>nonsmoking</dt>
  <dd>The `opentrip:nonsmoking` element's presence conveys that the person prefers a smoke-free environment. Specify as an empty entry. (e.g. &lt;nonsmoking/&gt;)</dd>
</dl>


### Mode Constructs

The optional `opentrip:mode` element is a container that represents additional information about the mode of transportation being used. The mode construct is also used to specify a preferred mode of transport, in which its content could be empty.


#### Examples

*  Example of a driver's mode of transportation:

    ```xml
    <t:mode kind="auto">
      <t:cost kind="USD">2.00</t:cost>
      <t:capacity>2</t:capacity>
      <t:vacancy>1</t:vacancy>
      <t:make>Tesla</t:make>
      <t:model>Roadster</t:model>
      <t:year>2009</t:year>
      <t:color>Red</t:color>
      <t:lic>ABCD123</t:lic>
    </t:mode>
```

*   Example of a passenger's preferred modes of transport:

    ```xml
    <t:mode kind="auto"/>
    <t:mode kind="van"/>
    <t:mode kind="bus"/>
```


#### Attributes

<dl>
  <dt>kind</dt>
  <dd>Specifies a mode of transportation or a type of location. May be one of `auto` (car, truck, etc.), `taxi`, `van` (vanpool), `bus`, `rail`, `ferry`, `bike`, `walk`, `air` (airplane), `slug` (slug-line or casual carpool pickup spot), or `lot` (parking lot).</dd>
</dl>


#### Elements

The following elements are in the `opentrip` namespace:

<dl>
  <dt>cost</dt>
  <dd>Cost of trip as a real number. Optional `kind` attribute specifies the unit of currency based on the 3-letter codes from <a href="http://en.wikipedia.org/wiki/ISO_4217">ISO 4217</a>. Default currency type is based on the location of trip.</dd>
  <dt>capacity</dt>
  <dd>Total capacity of passengers in vehicle.</dd>
  <dt>vacancy</dt>
  <dd>Number of available spots remaining.</dd>
  <dt>make</dt>
  <dd>Make of vehicle.</dd>
  <dt>model</dt>
  <dd>Model of vehicle.</dd>
  <dt>year</dt>
  <dd>Year of vehicle.</dd>
  <dt>color</dt>
  <dd>Color of vehicle.</dd>
  <dt>lic</dt>
  <dd>License plate of vehicle.</dd>
</dl>


## Contributors

[Carl Gorringe](http://carl.gorringe.org) - author


## License

This work is licensed under the [Creative Commons Attribution-Share Alike 3.0 United States License](http://creativecommons.org/licenses/by-sa/3.0/us/).
