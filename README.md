# the-algorithm
<article class="markdown-body entry-content container-lg" itemprop="text"><h1 tabindex="-1" dir="auto"><a id="user-content-twitter-recommendation-algorithm" class="anchor" aria-hidden="true" href="#twitter-recommendation-algorithm"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a>Twitter Recommendation Algorithm</h1>
<p dir="auto">The Twitter Recommendation Algorithm is a set of services and jobs that are responsible for constructing and serving the
Home Timeline. For an introduction to how the algorithm works, please refer to our <a href="https://blog.twitter.com/engineering/en_us/topics/open-source/2023/twitter-recommendation-algorithm" rel="nofollow">engineering blog</a>. The
diagram below illustrates how major services and jobs interconnect.</p>
<p dir="auto"><a target="_blank" rel="noopener noreferrer" href="/twitter/the-algorithm/blob/main/docs/system-diagram.png"><img src="/twitter/the-algorithm/raw/main/docs/system-diagram.png" alt="" style="max-width: 100%;"></a></p>
<p dir="auto">These are the main components of the Recommendation Algorithm included in this repository:</p>
<table>
<thead>
<tr>
<th>Type</th>
<th>Component</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr>
<td>Feature</td>
<td><a href="/twitter/the-algorithm/blob/main/src/scala/com/twitter/simclusters_v2/README.md">SimClusters</a></td>
<td>Community detection and sparse embeddings into those communities.</td>
</tr>
<tr>
<td></td>
<td><a href="https://github.com/twitter/the-algorithm-ml/blob/main/projects/twhin/README.md">TwHIN</a></td>
<td>Dense knowledge graph embeddings for Users and Tweets.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/trust_and_safety_models/README.md">trust-and-safety-models</a></td>
<td>Models for detecting NSFW or abusive content.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/src/scala/com/twitter/interaction_graph/README.md">real-graph</a></td>
<td>Model to predict likelihood of a Twitter User interacting with another User.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/src/scala/com/twitter/graph/batch/job/tweepcred/README">tweepcred</a></td>
<td>Page-Rank algorithm for calculating Twitter User reputation.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/recos-injector/README.md">recos-injector</a></td>
<td>Streaming event processor for building input streams for <a href="https://github.com/twitter/GraphJet">GraphJet</a> based services.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/graph-feature-service/README.md">graph-feature-service</a></td>
<td>Serves graph features for a directed pair of Users (e.g. how many of User A's following liked Tweets from User B).</td>
</tr>
<tr>
<td>Candidate Source</td>
<td><a href="/twitter/the-algorithm/blob/main/src/java/com/twitter/search/README.md">search-index</a></td>
<td>Find and rank In-Network Tweets. ~50% of Tweets come from this candidate source.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/cr-mixer/README.md">cr-mixer</a></td>
<td>Coordination layer for fetching Out-of-Network tweet candidates from underlying compute services.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/src/scala/com/twitter/recos/user_tweet_entity_graph/README.md">user-tweet-entity-graph</a> (UTEG)</td>
<td>Maintains an in memory User to Tweet interaction graph, and finds candidates based on traversals of this graph. This is built on the <a href="https://github.com/twitter/GraphJet">GraphJet</a> framework. Several other GraphJet based features and candidate sources are located <a href="/twitter/the-algorithm/blob/main/src/scala/com/twitter/recos">here</a></td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/follow-recommendations-service/README.md">follow-recommendation-service</a> (FRS)</td>
<td>Provides Users with recommendations for accounts to follow, and Tweets from those accounts.</td>
</tr>
<tr>
<td>Ranking</td>
<td><a href="/twitter/the-algorithm/blob/main/src/python/twitter/deepbird/projects/timelines/scripts/models/earlybird/README.md">light-ranker</a></td>
<td>Light ranker model used by search index (Earlybird) to rank Tweets.</td>
</tr>
<tr>
<td></td>
<td><a href="https://github.com/twitter/the-algorithm-ml/blob/main/projects/home/recap/README.md">heavy-ranker</a></td>
<td>Neural network for ranking candidate tweets. One of the main signals used to select timeline Tweets post candidate sourcing.</td>
</tr>
<tr>
<td>Tweet mixing &amp; filtering</td>
<td><a href="/twitter/the-algorithm/blob/main/home-mixer/README.md">home-mixer</a></td>
<td>Main service used to construct and serve the Home Timeline. Built on <a href="/twitter/the-algorithm/blob/main/product-mixer/README.md">product-mixer</a></td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/visibilitylib/README.md">visibility-filters</a></td>
<td>Responsible for filtering Twitter content to support legal compliance, improve product quality, increase user trust, protect revenue through the use of hard-filtering, visible product treatments, and coarse-grained downranking.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/timelineranker/README.md">timelineranker</a></td>
<td>Legacy service which provides relevance-scored tweets from the Earlybird Search Index and UTEG service.</td>
</tr>
<tr>
<td>Software framework</td>
<td><a href="/twitter/the-algorithm/blob/main/navi/navi/README.md">navi</a></td>
<td>High performance, machine learning model serving written in Rust.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/product-mixer/README.md">product-mixer</a></td>
<td>Software framework for building feeds of content.</td>
</tr>
<tr>
<td></td>
<td><a href="/twitter/the-algorithm/blob/main/twml/README.md">twml</a></td>
<td>Legacy machine learning framework built on TensorFlow v1.</td>
</tr>
</tbody>
</table>
<p dir="auto">We include Bazel BUILD files for most components, but not a top level BUILD or WORKSPACE file.</p>
<h2 tabindex="-1" dir="auto"><a id="user-content-contributing" class="anchor" aria-hidden="true" href="#contributing"><svg class="octicon octicon-link" viewBox="0 0 16 16" version="1.1" width="16" height="16" aria-hidden="true"><path d="m7.775 3.275 1.25-1.25a3.5 3.5 0 1 1 4.95 4.95l-2.5 2.5a3.5 3.5 0 0 1-4.95 0 .751.751 0 0 1 .018-1.042.751.751 0 0 1 1.042-.018 1.998 1.998 0 0 0 2.83 0l2.5-2.5a2.002 2.002 0 0 0-2.83-2.83l-1.25 1.25a.751.751 0 0 1-1.042-.018.751.751 0 0 1-.018-1.042Zm-4.69 9.64a1.998 1.998 0 0 0 2.83 0l1.25-1.25a.751.751 0 0 1 1.042.018.751.751 0 0 1 .018 1.042l-1.25 1.25a3.5 3.5 0 1 1-4.95-4.95l2.5-2.5a3.5 3.5 0 0 1 4.95 0 .751.751 0 0 1-.018 1.042.751.751 0 0 1-1.042.018 1.998 1.998 0 0 0-2.83 0l-2.5 2.5a1.998 1.998 0 0 0 0 2.83Z"></path></svg></a>Contributing</h2>
<p dir="auto">We invite the community to submit GitHub issues and pull requests for suggestions on improving the recommendation algorithm. We are working on tools to manage these suggestions and sync changes to our internal repository. Any security concerns or issues should be routed to our official <a href="https://hackerone.com/twitter" rel="nofollow">bug bounty program</a> through HackerOne. We hope to benefit from the collective intelligence and expertise of the global community in helping us identify issues and suggest improvements, ultimately leading to a better Twitter.</p>
<p dir="auto">Read our blog on the open source initiative <a href="https://blog.twitter.com/en_us/topics/company/2023/a-new-era-of-transparency-for-twitter" rel="nofollow">here</a>.</p>
</article>
