# Markdown Book

Storing Markdown Files on GitHub Repo: This involves creating a repository on GitHub and pushing your Markdown files to it.
Copying the Repo to a Computer: This step involves cloning the GitHub repository to your local computer.
Executing MkDocs Build Command: Here, you'll use MkDocs to convert the Markdown files into HTML for a website.
Pushing the Built Website to a Repo Branch: After building the HTML files, you push these back to a different branch of your GitHub repository.
GitHub Action to Serve the Site on GitHub Pages: Finally, you set up a GitHub Action in your repository that automatically deploys the website to GitHub Pages whenever the specified branch is updated.

```mermaid
graph TD
    A[Start: Create GitHub Repo] -->|Push markdown files| B[GitHub Repository with Markdown Files]
    B -->|git clone| C[Clone Repo to Local Computer]
    C -->|Run mkdocs build| D[Build HTML Using MkDocs]
    D -->|Push HTML to specific branch| E[Update GitHub Repo with Built Site]
    E -->|Set up GitHub Action| F[GitHub Action for GitHub Pages]
    F -->|Automate deployment| G[Deploy Site on GitHub Pages]
    G --> H[End: Website Live on GitHub Pages]

    classDef default fill:#f9f,stroke:#333,stroke-width:2px;
    classDef process fill:#bbf,stroke:#f66,stroke-width:2px, color:#fff;
    class A,B,C,D,E,F,G,H default;
    class B,C,D,E,F process;
```
