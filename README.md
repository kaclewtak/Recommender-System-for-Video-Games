# Recommender System for Video Games - Project Summary
Data mining project revolving around understanding 50 million+ steam reviews and creating a recommender engine.

This project seeks to create a recommender system using the review data collected from the Steam storefront. This data is current to the year 2024 and contains entries from as far back as 2010. The primary benefit of this system would be to accurately provide game recommendations based on a variety of factors. Accurate recommendations can lead to improved game sales with secondary benefits including customer retention, customer satisfaction, and platform growth.

Appropriate steps were taken to reduce and transform necessary data, before creating condensed representations, "ratings", of the various variables that would later be used in the recommender systems.

Throughout this project I found that despite the abundance of the data, the two proposed methods of creating a recommender system, Collaborative Filtering with SVD and Graph Based recommenders, are not sufficient in their current state. The foremost reason for their lack of quality can be rooted to an overwhelming amount of positive reviews for niche games, resulting in many games with a near perfect 100% upvote rating. In reality, we would not want to recommend such games to everyone and therefore steps were taken to remediate this issue. Additionally, the sheer quanitity of information present in this dataset made it difficult to assign a meaningful "rating" value to any particular review.

The written portion of the reviews were ignored for this project in order to contain scope, but it would be recommended that future endeavors attempt to include that information as it is highly informative.

All told, this project showcases the challenges of creating recommender systems from large scale datasets.

# Description of Methods

**Data Cleaning & Exploratory Data Analysis**: Given the constraints of my local hardware, I worked to reduce the overall dataset size first to an English only dataset (100+ million -> 50+ million), then reduced again by only taking more recent reviews past 2020 (50+ million -> 25+ million). This reduced dataset would be used going forward. While there are 23 total columns in this dataset, only a select subset of 12 are used: 'author_playtime_last_two_weeks', 'author_playtime_at_review', 'votes_up', 'voted_up', 'votes_funny', 'weighted_vote_score', 'steam_purchase', 'received_for_free', 'written_during_early_access', 'comment_count','author_num_games_owned', 'author_num_reviews'.

I choose those variables as I believe they best provide interaction values from which to create the recommender system from.

For the EDA process I perform both a univariate and multivariate analysis of the data to understand how the data behaves on its own and how each variable interacts with others. I took several remediation steps, which were informed by Variance Inflation Factors and Q-Q plots. I performed box-cox transformations on variables which needed transformation the most, with the remainder of the data unchanged.

**Initial Modeling**: This involved a two step process to create a final SVD-type recommender. The first step was to create a latent representation by condensing the aforementioned variables into a single representatation. Dimension reduction was performed using the following methods: PCA, KMeans, Autoencoder, Manual Computation. With these four scores I then created an SVD recommender using the Surprise package. While the modeling process yielded low RMSE and MAE scores, my conclusion is that this recommender system does not perform as expected.

Inspecting which games the random user has previously reviewed, we can see that they appear to enjoy First-Person Shooter (FPS) games and adventure games. With this in mind, the recommendations that have been made only somewhat make sense, lending further evidence that the low RMSE and MAE scores are not indicative of actual performance. I find that the recommender system suggests a wider variety of game genres, some of which might not be of particular interest to this user.

This is indicative of underlying problems with this model. In some ways, this was to be expected. Taking 12 variables, creating a combined representation of them, then taking the latent features of that combination will inevitably lead to information loss.

However, the variable compression must occur for this particular method to work. We can only pass the users, the items, and a single ratings value into SVD. While this is a valiant first effort, I recognize that there are many shortcomings. As such we must look to other methods to perhaps improve on the SVD recommender.

**Graph Based Recommender**: For this section I create a bipartite graph network utilizing the NetworkX package. A bipartite graph is chosen as the nature of the data lends itself to being split into users, items, and the edges being the aggregated "rating" score. A caveat to this section is that the dataset was again reduced to only 10% of the original size, as modeling never finished with the original dataset.

The only meaningfil result I was able to achieve was to retrieve the top items which were similar to a given item, which appears to work well, and the top items which are recommended for a particular user, which also appears to work well. However it must be noted that there is no meaningful metric to backup my claims for this, only industry knowledge and insights.

# Conclusions

In conclusion, these recommender systems still require much work before they could be even considered passable. While the SVD recommenders were able to produce consistent results, they remain inflexible and highly reliant on the "rating" representation between the users and the items. I suspect that the choice to stick with the SVD method was not wise and that other methods should have been considered once there was evidence of recommender failure.

The graph based recommender, while showing better recommendations, proved difficult to measure in regards to RMSE and MAE metrics. I nonetheless believe it justified to rely on a bipartite graph for this particular problem as we are mapping users to items. Perhaps other methods would also work, but further research would be required.

# Future Work

One large aspect of the data that has been ignored thus far are the written reviews. This would be an excellent source for NLP and including the sentiment value of such a method into the recommender system.

Explore other recommender methods, such as Wide-and-Deep, Variational Auto Encoders, and GGNN's. Such methods would be able to make use of the raw data without the need for aggregation and would allow for potentially better recommendations and results.
