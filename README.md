# Neighborhood-recommender-brief
Brief of a Canada-Wide neighborhood recommendaiton enging

The Canada-Wide Neighborhood Recommendation Engine covers 3,633 neighborhoods,
helping homebuyers quantify lifestyle trade-offs across competing criteria.
Each neighborhood and user profile is represented as a feature vector across 20+ dimensions
sourced from Local Logic and MLS APIs: neighborhood type (urban, trendy, suburban, rural),
amenity density (grocery, coffee, parks, transit, schools), transportation modes (walkability,
cycling, driving), and home characteristics. Although I built and tested autoencoder
representations, the regulatory environment made the choice clear: black-box neural
embeddings require compliance reviews that would have blocked production in a regulated
financial institution. Similarity is instead computed as a weighted dot product normalized
0-100%, with top contributing features extracted per recommendation, so risk, legal, and
product teams could directly audit what drove each match.

Budget handling operates in three modes: a hard budget ceiling, a soft penalty approach using
a λ-weighted decay function, and a bucketed affordable/stretch/unaffordable tier system.
Commute filtering uses distance-based calculations from a user-entered origin.

K-means and t-SNE clustering revealed consistent Canadian urban archetypes: Core-Urban
(high-density commercial integration, maximal walkability), Mid-Transit (balanced amenity
access, most sought-after by young professionals), and Residential Suburbs (high
home-ownership, distinct lack of third spaces like cafés and pubs).

A core data engineering challenge when sourcing the data: MLS and Local Logic boundaries had
no 1-1 relationship. I resolved this by querying the most specific Local Logic neighborhood per
MLS boundary using lat/long points. I designed a caching layer to avoid redundant API calls and
determined refresh intervals, production decisions I drove independently
