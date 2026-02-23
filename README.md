#!/bin/bash

# Check arguments
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <cloudfront-domain> <pipeline-name>"
    echo "Example: $0 dsikuhnjgrtfw.cloudfront.net my-pipeline-name"
    exit 1
fi

DOMAIN=$1
PIPELINE=$2

# Strip https:// if user provided it
DOMAIN=$(echo $DOMAIN | sed 's|https://||' | sed 's|/||g')

echo "---"
echo "### 📊 Live Stage Monitoring ($PIPELINE)"
echo ""
echo "| Stage | Activity | Current Status | Last Updated | 🔑 Commit | 👤 Author |"
echo "| :--- | :--- | :--- | :--- | :--- | :--- |"

# List of stages to generate rows for
STAGES=("Source" "Build" "DevDeploy" "StagingDeploy" "UATDeploy" "ProductionDeploy")
LABELS=("01. Source" "02. Build" "03. Dev" "04. Staging" "05. UAT" "06. Production")
ACTIVITIES=("📡 Listener" "🏗️ Compile" "🧪 Deploy" "🚀 Deploy" "🚥 Review" "💎 Live")

for i in "${!STAGES[@]}"; do
    STAGE=${STAGES[$i]}
    LABEL=${LABELS[$i]}
    ACT=${ACTIVITIES[$i]}
    
    # Path format: domain/pipeline/pipeline-stage.svg
    BASE_URL="https://$DOMAIN/$PIPELINE/$PIPELINE-$STAGE"
    
    echo "| **$LABEL** | $ACT | ![Status]($BASE_URL.svg) | ![Time]($BASE_URL-timestamp.svg) | ![Commit]($BASE_URL-commitId.svg) | ![Author]($BASE_URL-author.svg) |"
done

echo ""
echo "---"
echo "<sub>*Status badges feature live **animated progress bars** during active deployments.*</sub>"
