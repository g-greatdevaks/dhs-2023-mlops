# dhs-2023-mlops

A repository containing workshop material used during **DataHack Summit 2023**, organized by **Analytics Vidhya**, for the workshop titled **Mastering MLOps – The Art of Productionalizing ML Models**.

## Workshop Details

Machine Learning has become an integral part of businesses across all industries, from healthcare and finance to retail and manufacturing. However, developing and deploying Machine Learning models at scale can be a complex and challenging task. This is where Machine Learning Operations (MLOps) comes in. MLOps is an emerging field that aims to streamline the process of developing and deploying Machine Learning models by applying DevOps principles to Machine Learning workflows.

Throughout this 8-hour comprehensive workshop, we’ll guide you into the evolving realm of Machine Learning Operations (MLOps). We’ll delve into a deep understanding of its architecture, demonstrating the blueprint for successful implementation. We’ll shed light on its practical applications, demonstrating hands-on exercises with Natural Language Processing (NLP) and Computer Vision models. Emphasizing on practical knowledge, we will cover the deployment of MLOps using these data models, intended to bolster your productivity. Additionally, we’ll ensure you’re equipped with an MLOps toolkit, knowledge to pick the right platform, and are aware of the standard best practices for MLOps mastery. The workshop also aims to bust myths associated with MLOps, providing a comprehensive and clear understanding of this important field.

### Workshop Highlights

#### Module 1: Succinct Prelude to MLOps

- What MLOps Is and Why It Matters
- Busting The Buzzwords: DevOps, AIOps, and MLOps
- Lifecycle of a Machine Learning Model
- Different levels in MLOps Deployment

#### Module 2: The MLOps Sackmesser

- MLOps Toolkits for
  - Model Development
  - Tuning
  - Deployment
  - Monitoring

#### Module 3: MLOps and Real World Scenarios

- [Hands-on: MLOps Deployment on a Structured Dataset](./notebooks/01-mlops-deployment-structured-dataset/01-mlops-deployment-structured-dataset.ipynb)
- [Hands-on: MLOps Deployment with NLP](./notebooks/03-mlops-deployment-nlp/03-mlops-deployment-nlp.ipynb)
  - Bonus: Brief Introduction to Foundation Models and Prompt Engineering
- [Hands-on: MLOps Deployment with Computer Vision](./notebooks/02-mlops-deployment-computer-vision/02-mlops-deployment-computer-vision.ipynb)

#### Module 4: MLOps Governance

- Feedback Loop
- Maturity
- Data and Process Governance

#### Module 5: From Here to Where

- Power Up Your Productivity
- Learn to Pick the Right Platform
- Standard Practices for MLOps Mastery

### Pre-requisites

- System Requirement and Setup
  - Laptop with at least 8 GB RAM
  - Linux OS (preferred OS)
  - GitHub Account
  - Google Colab
  - [Docker Desktop installation](https://docs.docker.com/desktop/install/mac-install/) (install the package as per your OS)
    - Will be good to have 8 GB RAM and 8 GB Storage available
  - [Google Cloud Platform Free Trial - $300 Credits](https://cloud.google.com/free)
- Pre-reads
  - Familiarity with Python
  - Understanding of Docker and Containers

**Note:** For installation and configuration steps for the pre-requisites, follow the [notebooks/00-mlops-prerequisites/setup.md](/notebooks/00-mlops-prerequisites/setup.md) guide.

### Promotional Video and Registration

- Check out the promotional video [here](https://player.vimeo.com/video/845860397?h=26d0f117a1).
- Register for the workshop [here](https://www.analyticsvidhya.com/datahack-summit-2023/workshop/mastering-mlops-from-concepts-to-implementation/).

## Speaker Details

Anmol Krishan Sachdeva</br>
Hybrid Cloud Architect, Google</br>
[LinkedIn@greatdevaks](https://www.linkedin.com/in/greatdevaks) | [Twitter@greatdevaks](https://www.twitter.com/greatdevaks)

## Credits and References

- The demonstrations delivered as part of this workshop are inspired by the official [MLRun Demos](https://github.com/mlrun/demos/tree/1.2.x).
- The illustrative diagrams are inspired from the [Google MLOps Whitepaper](https://cloud.google.com/resources/mlops-whitepaper).

## Disclaimer

The content and views presented during the talk/session are author’s own and not of the organizations/companies they are associated with.

The workshop relies on using GCP Free Trial $300 Credits for demonstrations. It is highly recommended to [stay on top](https://cloud.google.com/free/docs/free-cloud-features#monitor-costs) of the cost and charges being incurred, and the credits being used/left. The attendees are encouraged to destroy the complete workshop related GCP setup after the workshop is concluded, to avoid incurring any additional unnecessary costs. The speaker nor any of the organizations / professional bodies / institutes the speaker is part of / associate with will be responsible for a situation where the attendee forgets to delete the resources created during the workshop and incurs any cost afterwards. The attendees understand that they will be solely responsible for the billing associated to GCP workloads, if any appears. The workshop is just 7-8 hours long and only needs one small sized GKE Cluster (only one 8 Core and 16 GB Worker Node is required; some small Persistent Volumes will be created as well).
