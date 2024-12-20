# Gemini 1.0 Pro Playground Application

## Introduction
This project demonstrates the capabilities of the Gemini 1.0 Pro models using **Google Cloud Vertex AI**. The application is a web-based playground built with **Streamlit**, offering multiple tabs for generating text, analyzing images, processing videos, and performing advanced AI tasks.

## Features
- **Story Generation**: Create customized stories using text prompts.
- **Marketing Campaigns**: Generate comprehensive marketing plans.
- **Image Playground**:
  - Furniture Recommendations
  - Oven Instructions
  - ER Diagram Analysis
  - Math Reasoning
- **Video Playground**:
  - Video Descriptions
  - Video Tags
  - Video Highlights
  - Video Geolocation

## Setup and Deployment

### Prerequisites
- Google Cloud Platform (GCP) account
- Vertex AI enabled on the GCP project
- Cloud Shell access
- Basic knowledge of Python and Docker

### Steps to Run Locally

1. **Clone the repository**:
   ```bash
   git clone <repository-url>
   cd gemini-app
   ```

2. **Activate Cloud Shell**:
   - Launch Cloud Shell from the GCP Console.
   - Run the following commands to configure your environment:
     ```bash
     PROJECT_ID=$(gcloud config get-value project)
     REGION=<your-region>
     echo "PROJECT_ID=${PROJECT_ID}"
     echo "REGION=${REGION}"
     ```

3. **Enable Required APIs**:
   ```bash
   gcloud services enable cloudbuild.googleapis.com cloudfunctions.googleapis.com run.googleapis.com logging.googleapis.com storage-component.googleapis.com aiplatform.googleapis.com
   ```

4. **Set Up Application Environment**:
   ```bash
   mkdir ~/gemini-app
   cd ~/gemini-app
   python3 -m venv gemini-streamlit
   source gemini-streamlit/bin/activate
   ```

5. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

6. **Run the Application locally**:
   ```bash
   streamlit run app.py \
   --browser.serverAddress=localhost \
   --server.enableCORS=false \
   --server.enableXsrfProtection=false \
   --server.port 8080
   ```
   Access the app via the provided URL or by using the Cloud Shell Web Preview on port 8080.

### Deployment on Cloud Run

1. **Set Up Docker Repository**:
   ```bash
   gcloud artifacts repositories create gemini-app-repo --location=$REGION --repository-format=Docker
   gcloud auth configure-docker "$REGION-docker.pkg.dev"
   ```

2. **Build the Docker Image**:
   ```bash
   gcloud builds submit --tag "$REGION-docker.pkg.dev/$PROJECT_ID/gemini-app-repo/gemini-app-playground"
   ```

3. **Deploy to Cloud Run**:
   ```bash
   gcloud run deploy gemini-app-playground \
     --port=8080 \
     --image="$REGION-docker.pkg.dev/$PROJECT_ID/gemini-app-repo/gemini-app-playground" \
     --allow-unauthenticated \
     --region=$REGION \
     --platform=managed \
     --project=$PROJECT_ID \
     --set-env-vars=PROJECT_ID=$PROJECT_ID,REGION=$REGION
   ```

4. **Access the Application**:
   Visit the URL provided after the deployment is complete.

## Application Tabs

### Tab 1: Story Generator
- Create stories based on user inputs like character name, persona, location, and plot type.

### Tab 2: Marketing Campaign
- Generate structured marketing strategies based on target audience, budget, and goals.

### Tab 3: Image Playground
- Use AI to analyze images for recommendations, instructions, and insights.
  - **Furniture Recommendation**: Match chairs to a living room image.
  - **Oven Instructions**: Extract instructions for resetting a clock from an oven control panel.
  - **ER Diagram Analysis**: Document entities and relationships from an ER diagram.
  - **Math Reasoning**: Interpret mathematical equations and provide explanations.

### Tab 4: Video Playground
- Extract insights from videos using AI.
  - **Video Description**: Summarize video content.
  - **Video Tags**: Generate tags for videos.
  - **Video Highlights**: Identify key moments and summarize content.
  - **Video Geolocation**: Determine location details based on video content.

## Requirements
- Python 3.8+
- Streamlit
- Vertex AI SDK for Python
- Google Cloud Logging

## Credits
- Developed by [Your Name/Team Name].
- Powered by Google Cloud Vertex AI.

## License
This project is licensed under the MIT License. See the LICENSE file for more details.
