```markdown
# Reddit Pushshift Database Analysis

## Summary

**S:** Unearth insights from Reddit.

**T:** Load and analyze the Reddit Pushshift database efficiently to uncover trends.

**A:** Using PSQL:
- Utilized pg_bulkload for efficient loading of 1 million tuples in under 250 seconds with parallelization.
- Implemented constraints including Foreign Keys (FK), Primary Keys (PK), and Composite Keys.
- Identified kindest, harshest, most active, and controversial subreddits.
- Experimented with fragmenting the large database using range and round-robin partitioning schemas.

**R:** 
- **Most Controversial:** Subreddits where the proportion of controversial comments (like/dislike ratio ~ 1) exceeds 1000. Includes topics like politics, AskReddit, announcements, and video games related to Minecraft and YouTubers.
  
- **Most Active:** Subreddits with the highest number of comments and highest scores (upvotes - downvotes).
  
- **Most Harsh:** Subreddits with the lowest average score, often mundane topics or NSFW subreddits that do not evoke strong feelings.
  
- **Most Kindest:** Subreddits with the highest average score, typically focused on feminine topics, women empowerment, and racing.

**Other Insights:**
- Analysis of users with the most karma.
- Examination of subreddits with the most comments and most upvoted/downvoted comments.
- Identification of most active subreddits based on posts per month.

--------------

# ETL - Credit Score Data

- **Features:**
  - Age
  - Number of Past Due 1-2 months
  - Number of Past Due 1-2 months but not worse
  - Number of Past Due 3 months
  - Income
  - Number of mortgage and real estate loan lines (Home loans)
  - Number of open credit (credit cards) and loan (home/car) lines
  - Number of Dependents
  - Revolving Utilization of Unsecured Lines: Total balance (due) on credit cards + other lines / (Sum(credit Limits))

- **Target:**
  - If user is delinquent - Defaults after 90 days.

- **Extraction:**
  - Load data from CSV in the data lake S3 bucket using AWS Glue Crawler to infer the schema of the database using a crawling process and extract the data to a Glue database in S3.

- **Transformation:**
  - Use PySpark Glue ETL jobs to remove duplicates, drop null values, and remove outliers. Specifically, remove records of individuals below 18 years old and those with revolving ratios > 1. Standardize the dataset with mean 0 and standard deviation 10.

- **Load:**
  - Save the dataset to the target DynamoDB (warehouse) and S3 bucket (data lake).
    
- **Insights from Athena and Quicksight:**
  - Mean debt ratio, standard deviation, and percentile scores.
  - Proportion of people who defaulted.
  - Revolving debt trend: younger people are more in debt; as age increases, debt decreases.
  - Monthly income bell curve from ages 18 to 90.
  - Dataset imbalance: more individuals between 40-50 years old and more individuals with debt exceeding 90% compared to others.

--------------

# Kerner Lab

**Current Work:** Benchmarking vision models on Mars datasets.

**S:**
- The Mars HiRISE rover rotates over the land's surface and captures images.
- Cones are geological landforms of importance due to the presence of water.
- Cones are not segmented or detected on the Mars surface.

**T:**
- Segment crops using a segmentation model.
- Perform tile-wise segmentation and combine the segmentation maps.
- Curate the dataset through a domain expert.

**A:**
- Used an ensemble (voted) of segmentation models: Deeplabv3, MANET, UNET, UNET++, and FPN for segmenting surface tiles of 512x512 pixels.
- Stitched the tiles with an overlap of 256x256 pixels on all sides to remove artifacts.
- Experimented by adding text-conditioning heads and performing a multi-modal fusion to add textual context through prompts.
- Generated prompts for images using a RAG-based approach on a large corpus of Mars-cones papers.

**R:**
- Established a baseline for segmenting cones on Mars and validated the applicability of multi-modal segmentation.

--------------

# CSE 598: ML for RS

**S:**
- Diffusion and language models lack context and understanding of domain-specific knowledge, particularly in remote sensing.

**T:**
- Create a language model and image generation model for remote sensing.

**A:**
- Finetuned a domain-specific Stable Diffusion v1.5 to generate images for remote sensing.
- Finetuned a domain-specific causal language model based on Bloom and Phi-1.5.
- Generated images for LULC (Land Use and Land Cover) with text from an LLM to create a dataset and finetuned it downstream.

**R:**
- Created a model with biased generations that look good in terms of quality but lack accuracy according to domain experts.
- Achieved an average perplexity score, but the model hallucinates a lot.
- More compute and data are needed, but the results are promising.

--------------

# SWE Intern

**S:**
- Create a search bar to reduce time when searching articles on the Rocket Homes Blog.

**T:**
- Design a search bar in Angular, crawl blog contents periodically, save it, and search it with GCS.

**A:**
- Deployed a Norconex web crawler with Terraform to crawl the articles and saved them to GCS.
- Created the search bar in Angular and used NestJS for the backend connections, including the search and suggestion API.

**R:**
- Reduced search time to 30 seconds for users.
- Increased user engagement by around 25% and supported the customer acquisition strategy.

**E:**
- Created an SD inpainting demo for customizing Rocket Homes listings.

--------------

# Teuvonet

**S:**
- Detect perpetrators/criminals with guns near school premises through CCTV cameras and raise an alarm (Genetech). Do not raise an alarm when there is a civilian or policeman (with or without a gun).

**T:**
- Used an XAI method where different body parts and guns are detected separately. Based on a weighted softmax score, raise an alarm. Focus on low-latency, accurate results with lightweight models on AWS.

**A:**
- Tried custom CNN, ResNet, EfficientNet, MobileNet, and finally settled on FastVIT/MobileVIT.
- Quantized the models and deployed them on an AWS EC2 instance.
- The container (GPU) ingested video, broke it into frames, and passed it to ONNX with a C++ interface for inference.
- This producer sends the images and classification results to topics that forward them to MongoDB (base64), a web UI (that employs the alarm logic), and results to S3.
- Started work with the same models on detecting military aircraft/vehicles for spacecrafts and used YOLOv5 for detecting these objects.

**R:**
- Achieved high F1 scores for the classification model. The new VIT models are lightweight with fast inference, reduced model size by 40%, and reduced false positives by 15%.
```
