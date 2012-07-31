## Semantic Web Crawling Visualization
Kirby Banman, <kdbanman@ualberta.ca>

# Important Bits

Examples and visualizations of semantic web crawls using:

Version 0.2:
- Semantic Crawler: [LDSpider 1.1e](http://code.google.com/p/ldspider/) revision 304 checked out for extension to .gexf output

Version 0.1:
- Graph Visualizer: [Gephi 0.8.1 beta 201202141941](http://gephi.org/) using plugin [HttpGraph 1.0.6](https://gephi.org/plugins/http-graph/)
- Semantic Crawler: [LDSpider 1.1e](http://code.google.com/p/ldspider/)

# Overview

*The initial motivation for this project was to produce [this video](http://www.youtube.com/watch?v=CCBvwWIba3c) and [this other video](http://www.youtube.com/watch?v=w9UKUpyqw_4).*

There are many unanswered questions regarding the nature of semantic web crawling, and this is my attempt to sharpen those questions with some visual depictions of different crawling strategies.  Different methods of crawling give rise to very different behaviour, which should be evident in the different image, graph, and video visualizations.

Version 0.2:

This version is an attempt to address the inadequacies of version 0.1 (see 0.1 analysis below).  More advanced crawl strategies will be implemented in an attempt to focus the crawl behaviour and results on a particular topic.  Insights and implementations of these strategies are explored in semantic [{dong}] and traditional [{menczer}][{diligenti}] web contexts.

- Each crawl strategy is given a subdirectory of ver0.2/
- Each subdirectory contains:
    1. A java executable for the crawl
    2. A text description of the crawl strategy
    3. An rdf+xml file of the crawl results
    4. A graph visualization of the crawl results in `.svg`, `.gml`, and `.gephi` formats
    5. (Tentative) A .gexf dynamic graph file for visualization of crawler behaviour

Version 0.1:

This version is a preliminary experiment to see the difference between the classic crawl strategies of depth-first and breadth-first, initially explored in 1994 [{pinkerton}][{de bra}], in the context of the semantic web.  It also represents an exploration of visaulization techniques and tools.

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

# Roadmap

Version 0.2:

##### Static: Visualize the RDF graph at the end of the crawl for different focused algorithms:
- get LDSpider to output RDF+XML using the API rather than the CLI
- use Gephi's Semantic Web Import to visualize the crawl results (may be able to do semi-dynamic vis by SPARQL queries and round-based named graphs)
- implement a few topically focused algorithms in LDSpider to visualize results of a control topic

##### Dynamic: Visualize the RDF graph as it's crawled, rather than the crawler's HTTP traffic:
- Extend LDSpider to output crawl results as a dynamic graph for visualization - .gexf format seems most applicable
- Revision 304 of LDSpider checked out for extension
- [gexf4j](https://github.com/francesco-ficarola/gexf4j) library may be the best way to author the dynamic graph files for output

Version 0.1:

##### A sufficient set of seed URIs, LDSpider settings, and Gephi settings could be found so that scripting the 'Human Procedure' could be done by command line or respective APIs:
- For each seed URI and for each link type followed:
- Crawl breadth- and depth- first to obtain similarly sized graphs (current strategy is to aim for less than 1000 nodes)
- Crawl breadth- and depth- first with a uniform set of configurations
- The uniform set of configurations could be 2 separate configs, one for a small graph and one for a larger graph, for each of breadth and depth
- Video visualizations are likely impractical for the uniform set, as the crawl time could become impractically large for some seed/link combinations.  For example, a depth-first crawl from Einstein's dbpedia URI, following only rdf:type links, has a Round 2 queue of ~290 URIs, then a Round 3 queue of ~57000 URIs, and the  maxuri parameter [is only checked at the end of every round](http://code.google.com/p/ldspider/source/browse/trunk/src/com/ontologycentral/ldspider/Crawler.java#358)

##### Especially after above scripting, split programmatic and configuration files from presentation files into separate directories.  In addition to sorting what's already there:
- Include crawler config in plain english in the visualization directories.
- Include a terminal log and the crawl output file.

# License

All scripts, images, and graphs are Copyright 2012 (C) Kirby Banman, <kdbanman@ualberta.ca>.

This content is licensed under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This content is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this content.  If not, see <http://www.gnu.org/licenses>.
