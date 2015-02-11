[![Stories in Ready](https://badge.waffle.io/tfredrich/gson-ext.png?label=ready&title=Ready)](http://waffle.io/tfredrich/gson-ext)

gson-ext provides JSON to XML conversion.  This is handy for such tasks as leveraging XPATH against a JSON string.  Because XML is more verbose than JSON and requires elements to be wrapped in matching tags,
gson-ext injects some tags into the generated XML where JSON does not.  There are primarily six cases (ignoring nesting. Nesting has the same six cases):

* Atomic Value (e.g. {"id":1005})
* Named Root (e.g. {"foo":{"bar":"bat", "bat":"boo"}}), where gson-ext uses the name as the root element (e.g. "<foo>").
* Unnamed Root (e.g. {"foo":"bar", "bat":"boo"}), where gson-ext injects "root" as the root element name (e.g. "<root>").
* Named Array (e.g. {"foo":[{"bar":"one"}, {"bar":"two"}, {"bar":"three"}]), where gson-ext uses the array element name as the name of the list, wrapping each element of the list in "<item>" tags.
* Unnamed Array (e.g. {[{"bar":"one"}, {"bar":"two"}, {"bar":"three"}]), where gson-ext uses the tag "<list>" as the name of the list, wrapping each element of the list in "<item>" tags.

The following examples should get you going:
Atomic Value
JSON:
	{"id":1005}

XML:
	<id>1005</id>

Named Root
JSON:
	{ "resource" : { "name" : "test post", "data" : "some data" } }

XML:
	<resource><name>test post</name><data>some data</data></resource>

Unnamed Root
JSON:
	{"access_token":"mauth|79889m9rwet|2114798|2010-06-07T09%3a51%3a03|66cb32d9e0cf9ea2dad1f999946af951","expires":3600}

XML:
	<root><access_token>mauth|79889m9rwet|2114798|2010-06-07T09%3a51%3a03|66cb32d9e0cf9ea2dad1f999946af951</access_token><expires>3600</expires></root>

Named Array
JSON:
	{"notifications":[{"id":1005,"subject":"foo"},{"id":1006,"subject":"bar"}]}

XML:
	<notifications><item><id>1005</id><subject>foo</subject></item><item><id>1006</id><subject>bar</subject></item></notifications>

Unnamed Array
JSON:
	[{"id":1005,"subject":"foo"},{"id":1006,"subject":"bar"}]

XML:
<list><item><id>1005</id><subject>foo</subject></item><item><id>1006</id><subject>bar</subject></item></list>

Named Array w/ Nested Unnamed Array
JSON:
	[{"named_list":[{"a":"a_value"}, {"b":"b_value"}, {"c":"c_value"}]},{"named_root":{"foo":"bar"}},{"a":"b","c":"d"},[{"humpty":1}, {"dumpty":2}]]

XML:
	<list><item><named_list><item><a>a_value</a></item><item><b>b_value</b></item><item><c>c_value</c></item></named_list></item><item><named_root><foo>bar</foo></named_root></item><item><a>b</a><c>d</c></item><item><list><item><humpty>1</humpty></item><item><dumpty>2</dumpty></item></list></item></list>

Unnamed Array w/ Nested Unnamed Array
JSON:
	[[{"a":"a_value"}, {"b":"b_value"}, {"c":"c_value"}, [{"humpty":1}, {"dumpty":2}]], [{"humpty":3}, {"dumpty":4}]]

XML:
	<list><item><list><item><a>a_value</a></item><item><b>b_value</b></item><item><c>c_value</c></item><item><list><item><humpty>1</humpty></item><item><dumpty>2</dumpty></item></list></item></list></item><item><list><item><humpty>3</humpty></item><item><dumpty>4</dumpty></item></list></item></list>

Named Heterogeneous Array w/ Nested Unnamed Array
JSON:
	[{"named_list":[{"a":"a_value"}, [{"humpty":1}, {"dumpty":2}], {"c":"c_value"}]},{"named_root":{"foo":"bar"}},{"a":"b","c":"d"}]

XML:
	<list><item><named_list><item><a>a_value</a></item><item><list><item><humpty>1</humpty></item><item><dumpty>2</dumpty></item></list></item><item><c>c_value</c></item></named_list></item><item><named_root><foo>bar</foo></named_root></item><item><a>b</a><c>d</c></item></list>

Unnamed Array w/ Nested Named Array
JSON:
	[{"named_list":[{"a":"a_value"}, {"nested_list":[{"humpty":1}, {"dumpty":2}]}, {"c":"c_value"}]},{"named_root":{"foo":"bar"}},{"a":"b","c":"d"}]

XML:
	<list><item><named_list><item><a>a_value</a></item><item><nested_list><item><humpty>1</humpty></item><item><dumpty>2</dumpty></item></nested_list></item><item><c>c_value</c></item></named_list></item><item><named_root><foo>bar</foo></named_root></item><item><a>b</a><c>d</c></item></list>