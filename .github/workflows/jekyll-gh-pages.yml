#!/bin/bash

# GitHub username and personal access token (replace with your own)
USERNAME="your-username"
TOKEN="your-personal-access-token"

# Function to create a new GitHub repository
create_repo() {
    echo "Creating new GitHub repository..."
    response=$(curl -s -X POST -H "Authorization: token $TOKEN" -d '{"name": "'"$REPO_NAME"'", "private": true}' "https://api.github.com/user/repos")
    if [[ ! "$response" =~ "html_url" ]]; then
        echo "Failed to create repository. Aborting."
        exit 1
    fi
    REPO_URL=$(echo "$response" | grep -o '"html_url": *"[^"]*"' | grep -o '"[^"]*"$' | tr -d '"')
    echo "Repository created successfully: $REPO_URL"
}

# Function to initialize the repository with a README.md file
init_repo() {
    echo "Initializing repository with README.md file..."
    response=$(curl -s -X PUT -H "Authorization: token $TOKEN" -d '{"message": "Initial commit", "content": "I'm just a README.md file."}' "https://api.github.com/repos/$USERNAME/$REPO_NAME/contents/README.md")
    if [[ ! "$response" =~ "html_url" ]]; then
        echo "Failed to initialize repository. Aborting."
        exit 1
    fi
    echo "Repository initialized successfully."
}

# Function to enable GitHub Pages
enable_pages() {
    echo "Enabling GitHub Pages..."
    response=$(curl -s -X POST -H "Authorization: token $TOKEN" -d '{"source": {"branch": "main", "path": "/"}, "repository": {"name": "'"$REPO_NAME"'", "owner": "'"$USERNAME"'"}}' "https://api.github.com/repos/$USERNAME/$REPO_NAME/pages")
    if [[ ! "$response" =~ "html_url" ]]; then
        echo "Failed to enable GitHub Pages. Aborting."
        exit 1
    fi
    PAGES_URL=$(echo "$response" | grep -o '"html_url": *"[^"]*"' | grep -o '"[^"]*"$' | tr -d '"')
    echo "GitHub Pages enabled successfully: $PAGES_URL"
}

# Main function
main() {
    create_repo
    init_repo
    enable_pages
    echo "Done! Your GitHub Pages site should be available shortly."
}

# Execute main function
main
