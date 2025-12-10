import requests
import os
import subprocess

# GitHub API URL for repository information
GITHUB_API_URL = "https://api.github.com/repos/"

# Function to clone a GitHub repository
def clone_repo(repo_url):
    try:
        print(f"Cloning repository from {repo_url}...")
        subprocess.run(["git", "clone", repo_url], check=True)
        print("Repository cloned successfully.")
    except subprocess.CalledProcessError:
        print("Error cloning the repository.")

# Function to get repository details from GitHub API
def get_repo_details(owner, repo_name):
    url = f"{GITHUB_API_URL}{owner}/{repo_name}"
    response = requests.get(url)

    if response.status_code == 200:
        repo = response.json()
        print(f"Repository: {repo['name']}")
        print(f"Description: {repo['description']}")
        print(f"Owner: {repo['owner']['login']}")
        print(f"Stars: {repo['stargazers_count']}")
        print(f"Forks: {repo['forks_count']}")
        print(f"Open Issues: {repo['open_issues_count']}")
    else:
        print(f"Error fetching details for {owner}/{repo_name}.")

# Main Program
if __name__ == "__main__":
    # Example usage
    owner = "octocat"  # GitHub user or organization name
    repo_name = "Hello-World"  # Repository name

    # Fetch and display repository details
    get_repo_details(owner, repo_name)

    # Clone the repository (if needed)
    repo_url = f"https://github.com/{owner}/{repo_name}.git"
    clone_repo(repo_url)
