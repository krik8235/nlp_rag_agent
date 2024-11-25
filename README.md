# Overview

An image-to-text agent system that retrieves, analyzes, and answers questions from the end user using NLP.

<!-- [Website](https://contract-neg-system.streamlit.app/)

![UI](https://res.cloudinary.com/dfeirxlea/image/upload/v1731425753/portfolio/fsqjubnndrawnp9ezimq.png)

![Terminal](https://res.cloudinary.com/dfeirxlea/image/upload/v1731425419/portfolio/mytqa9jtu8yexf6oc0k8.png) -->


## Table of Contents
*generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Overview](#overview)
  - [Table of Contents](#table-of-contents)
  - [Key Features](#key-features)
  - [Technologies Used](#technologies-used)
  - [Project Structure](#project-structure)
  - [Setup](#setup)
  - [Usage](#usage)
  - [Development](#development)
    - [Package Management with pipenv](#package-management-with-pipenv)
    - [Pre-commit Hooks](#pre-commit-hooks)
    - [Customizing AI Agents](#customizing-ai-agents)
    - [Modifying RAG Functionality](#modifying-rag-functionality)
  - [Contributing](#contributing)
  - [Troubleshooting](#troubleshooting)


## Key Features
Automate the contract review process through the following steps:

1. **Image Upload**:
   - Uploads an image (jpg/png) to the system.
   - Use NLTK's toolkit to encode the image by paragraph.

2. **Retrieve Key Information**:
   - Employs Llama 3.1 (running on Together AI)
   - Extract necessary information on reading paragraph, questions, and correct answers.
   - Return it in string format.

3. **AI-Powered Vocabulary List**:
   -  Send the retrieved string data to the agent
   -  Genearate vocabularies in the text and store them in the csv file

4. **User Interaction**:
   - Present the vocabulary list for the user (This repository only contains a simple file uploader as an user interface.)




## Technologies Used
[data-doc-management]

   - Chroma DB: Vector database for storing and querying standard contract clauses
   - SQLite: Database for storing application data

[ai-model-curation]

   - Together AI: Hosting Llama 3.1 for text processing, clause segmentation, and response generation
   - AIML API: Curation platform to access AI models and other tasks

[task-handling]

   - NLDK: Natural language toolkit for building Python programs to work with human language data [Doc](https://www.nltk.org/)

[deployment-framework]

   - Python: Primary programming language. We use ver 3.12
   - Flask: Web framework for the backend API
   - [Flask Cors](https://pypi.org/project/Flask-Cors/): A Flask extension for handling Cross Origin Resource Sharing (CORS), making cross-origin AJAX possible
   - pipenv: Python package manager
   - pre-commit: Managing and maintaining pre-commit hooks

   - React: Frontend framework
   - Vercel: User endpoint


## Project Structure

```
.
├── __init__.py
├── app.py                  # Flask application
├── agents.py               # Define the  AI agents
├── Prompts/                # Store prompt and system context templates
│   ├── System.py
│   └── User.py
│   └── ...
├── db/                     # Database files
│   ├── chroma.sqlite3
│   └── ...
└── sample_textbook_images/ # Sample textbook images for the test 
└── uploads/                # Uploaded image files
```

## Setup

1. Install the `pipenv` package manager:
   ```
   pip install pipenv
   ```

2. Install dependencies:
   ```
   pipenv shell
   pipenv install -r requirements.txt -v
   ```
   * in case of error - `pip install -r requirements.txt`

3. Set up environment variables:
   Create a `.env` file in the project root and add the following:
   ```
   TOGETHER_API_KEY=your_together_api_key
   ```

## Usage
1. Test the AI assistant:
   ```
   pipenv shell
   python main.py
   ```
   In the terminal, you can trace the process analyzing the sample textbook data.

2. Start the Flask backend:
   ```
   python -m flask run --debug
   ```
   The backend will be available at `http://localhost:5000`.


3. In a separate terminal, run the React frontend app:
   ```
   cd frontend
   npm start
   ```
   The frontend will be available at `http://localhost:3000`.

   Call the Flask API from the frontend app to see the result on user interface.



## Development

### Package Management with pipenv

- Add a package: `pipenv install <package>`
- Remove a package: `pipenv uninstall <package>`
- Run a command in the virtual environment: `pipenv run <command>`

* After adding/removing the package, add/delete the package on/from `requirements.txt` accordingly

* To reinstall all the dependencies, delete `Pipfile` and `Pipfile.lock`, then run:
   ```
   pipenv shell
   pipenv install -r requirements.txt -v
   ```


### Pre-commit Hooks

1. Install pre-commit hooks:
   ```
   pipenv run pre-commit install
   ```

2. Run pre-commit checks manually:
   ```
   pipenv run pre-commit run --all-files
   ```

Pre-commit hooks help maintain code quality by running checks for formatting, linting, and other issues before each commit.

*To skip pre-commit hooks
   ```
   git commit --no-verify -m "your-commit-message"
   ```

### Customizing AI Agents

To modify or add new AI agents, edit the `agents.py` file. Each agent is defined with a specific role, goal, and set of tools.

To modify or add templated prompts, edit/add files to the `Prompts` folder.


### Modifying RAG Functionality

The system uses Chroma DB to store and query the images uploaded. To update the knowledge base:

1. Add new contract documents to the `uploads/` directory.
2. Modify the `agents.py` file to update the ingestion process if necessary.
3. Run the ingestion process to update the Chroma DB.


## Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/your-amazing-feature`)
3. Commit your changes (`git commit -m 'Add your-amazing-feature'`)
4. Push to the branch (`git push origin feature/your-amazing-feature`)
5. Open a pull request



## Troubleshooting

Common issues and solutions:
- API key errors: Ensure all API keys in the `.env` file are correct and up to date. (See `.env.init`)
- Database connection issues: Check if the Chroma DB is properly initialized and accessible.
- Memory errors: If processing large contracts, you may need to increase the available memory for the Python process.
- Issues related to the AI agents or RAG system: Check the `output.log` file for detailed error messages and stack traces.