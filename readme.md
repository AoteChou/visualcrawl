## Semantic Web Crawling Visualization
Kirby Banman, <kdbanman@ualberta.ca>

# Important Bits

Examples and visualizations of semantic web crawls using:

### Version 0.2:
- Graph Visualizer: [Gephi 0.8.1 beta 201202141941](http://gephi.org/) using plugin [SemanticWebImport](https://gephi.org/plugins/semanticwebimport/)
- Semantic Crawler: [LDSpider 1.1e](http://code.google.com/p/ldspider/) revision 304 checked out for extension to .gexf output using the [gexf4j 0.4.0](https://github.com/francesco-ficarola/gexf4j) library
- Triplestore Instance: [4Store 1.1.3](http://4store.org/trac/wiki/Download)

### Version 0.1:
- Graph Visualizer: [Gephi 0.8.1 beta 201202141941](http://gephi.org/) using plugin [HttpGraph 1.0.6](https://gephi.org/plugins/http-graph/)
- Semantic Cawler: [LDSpider 1.1e](http://code.google.com/p/ldspider/)

# Overview

*The initial motivation for this project was to produce [this video](http://www.youtube.com/watch?v=CCBvwWIba3c) and [this other video](http://www.youtube.com/watch?v=w9UKUpyqw_4).*

There are many unanswered questions regarding the nature of semantic web crawling (see [SemCrawl.md](visualcrawl/docs/SemCrawl.md) in docs directory), and this is my attempt to sharpen those questions with some visual depictions of different crawling strategies.

### Version 0.2:

This is an attempt to address the inadequacies of version 0.1 (see 0.1 analysis below). Rather than visualizing a dynamic graph of the HTTP traffic of the crawler, one may visualize the RDF graph that the crawler aggregates as it crawls.  There is a way to visualize just the final results of the crawl, as well as a dynamic graph of the crawl results.
*Advanced crawl strategies were not implemented.  However, a breadth-first crawl was implemented with the option to have a sorted and unsorted URI queue, and the static and dynamic RDF visualizations were made to work.*

A folder will be created in a directory called 'crawls' for each crawl, containing:

- accessLog.txt - an HTTP error log
- redirectLog.nx - a log of HTTP redirects (as a list of URI pairs)
- seed.txt - a text file containing the seed used for the crawl
- results.gexf - (if -dynamic mode is used) a dynamic graph file of the crawl results

##### Static:

java -jar BreadthVis.jar -static [endpoint] [seed] [rounds] [breadth] [frontier]

- [endpoint] - SPARQL endpoint update URI
- [seed] - crawl seed URI
- [rounds] - breadth-first crawl depth
- [breadth] - URIs per crawl round
- [frontier] - either -sorted or -unsorted frontier (URIs sorted by inlink degree every round)

The basic idea here is that the crawl results are fed into a triplestore as a unique named graph for each crawl.  To visualize, Gephi's SemanticWebImport plugin can be used to query the triplestore using a CONSTRUCT query for all triples under the specific crawl's named graph.

##### Dynamic:

java -jar BreadthVis.jar -dynamic [filename] [seed] [rounds] [breadth] [frontier]

- [filename] - .gexf output filename
- [seed] - crawl seed URI
- [rounds] - breadth-first crawl depth
- [breadth] - URIs per crawl round
- [frontier] - either -sorted or -unsorted frontier (URIs sorted by inlink degree every round)

The .gexf results file is a dynamic, directed graph of the RDF graph that the crawler discovered.  You can [read about .gexf here](http://gexf.net/format/).  The nodes of the graph are the RDF entities (subjects or objects) and the edges of the graph ore the RDF predicates.  Each node also has an attribute `timesCrawled` which is the number of times the RDF entity was encountered during the crawl.

#### In Gephi

When the crawl is complete, simply open the .gexf file containing the crawl results in Gephi (-dynamic crawl) or use the Semantic Web Import plugin to import the results graph from the SPARQL endpoint (-static crawl).  The graph will be in Gephi's default square layout, here's how I make the structure apparent:

1. In the `Ranking` window, color the nodes according to one of the attributes (I choose `OutDegree`)
1. In the `Ranking` window, scale the nodes' size according to another attribute (I choose `timesCrawled`, min=6, max=50, linear spline)
1. In the `Layout` window, choose `ForceAtlas 2` and increase the `Tolerance (speed)` to ~2 and run until semi-stable hubs form, then enable the `Prevent Overlap` option and stop the algorithm after a few more seconds.
1. Visibility of node labels and edge labels can be toggled with buttons below the graph, but unless they are scaled appropriately, the options will slow down the rendering significantly

###### Dynamics

1. To view the graph dynamically and watch the crawl results grow as they were discovered, enable the timeline option.
1. In the timeline options menu -> Set time format... ensure `numeric` is selected.
1. In the timeline options menu -> Set play settings... ensure `Mode` is set to `One bound`
1. In the timeline options menu -> Set custom bounds... it may be necessary to click `Reset defaults`
1. You may now drag the right bound of the timeline slice forward and backward on the time scale and the graph will appear in the layout you configured above

### Version 0.1:

This is a preliminary experiment to see the difference between the classic crawl strategies of depth-first and breadth-first, initially explored in 1994 [{pinkerton}][{de bra}], in the context of the semantic web.  It also represents an exploration of visaulization techniques and tools.

- Crawl settings are described in the shell scripts.
- Generated graphs show the dereferenced URIs as nodes and the links between URIs as edges.
- The real-time generation of the graph is shown at the linked video for each crawl.
- The crawl settings are adjusted so that the complete activity graph contains less than 1000 nodes.

##### Script Procedure

Each script in ver0.1/:

1. Generates directory of the same name for:
    - Associated visualisation images and graph files 
    - Text file containing link to crawl visualization video (if present)
    - Crawl results (if any)
2. Populates the seed text file with the seed URI(s)
3. Starts the crawler with the options described in the script header

##### Human Procedure

1. Gephi/HttpGraph are set up to generate a graph according to HTTP traffic routed through `http://localhost:8088`
2. Script is run, starting the crawler with traffic routed through the proxy
3. When the crawl is done, the resulting graph is altered for appearance and saved in `.svg`, `.gml`, and `.gephi` formats

> Analysis:  The strategies give rise to very different results and behaviour (see pics/videos).
> Using the HTTP traffic to visualize the crawl behaviour was good as an initial experiment, but visualizing the RDF graph as it is aggregated would be much more relevant and would eliminate the inclusion of the `localhost` node.
> A dynamic graph document would be much more suitable for analysis of crawl behaviour than a video.  The graph could be explored by the viewer as they see fit, rather than the naive presentation of the graph as it forms for the author.

# License

All software used in this project that is not my own is licensed as open source.

All content authored by me licensed under the terms of the GNU General Public License as published by the Free Software Foundation, either  3 of the License, or (at your option) any later gc.

This content is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License in the docs directory.  If not, see <http://www.gnu.org/licenses>.
