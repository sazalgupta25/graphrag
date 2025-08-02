# GraphRAG Repository - Comprehensive Usage Guide

## What is GraphRAG?

GraphRAG (Graph Retrieval-Augmented Generation) is a sophisticated data pipeline and transformation suite developed by Microsoft Research. It's designed to extract meaningful, structured data from unstructured text using the power of Large Language Models (LLMs), specifically leveraging knowledge graph memory structures to enhance LLM outputs on private data.

## Core Purpose and Value Proposition

GraphRAG addresses a fundamental challenge in AI: how to make Large Language Models more effective at reasoning about your private, domain-specific data. Traditional RAG (Retrieval-Augmented Generation) approaches often fall short when dealing with complex, interconnected information. GraphRAG solves this by:

1. **Creating Knowledge Graphs**: Converting unstructured text into structured knowledge graphs that capture entities, relationships, and communities
2. **Enhanced Retrieval**: Using graph-based retrieval methods that understand context and relationships
3. **Improved Reasoning**: Enabling LLMs to perform better reasoning over interconnected data
4. **Scalable Processing**: Providing a robust pipeline for processing large volumes of text data

## Key Features and Capabilities

### 1. Data Indexing Pipeline
- **Text Processing**: Sophisticated text parsing and entity extraction
- **Graph Construction**: Automatic creation of knowledge graphs from unstructured data
- **Community Detection**: Identification of related entity clusters
- **Hierarchical Summarization**: Multi-level summarization of document collections

### 2. Advanced Query System
- **Global Search**: Queries that leverage the entire knowledge graph structure
- **Local Search**: Focused queries on specific entities or relationships
- **Hybrid Approaches**: Combination of traditional and graph-based retrieval methods

### 3. LLM Integration
- **OpenAI Integration**: Built-in support for OpenAI models (GPT-3.5, GPT-4)
- **Azure Services**: Deep integration with Azure AI services
- **Prompt Engineering**: Advanced prompt tuning capabilities for optimal results

### 4. Vector Store Support
- **Multiple Backends**: Support for various vector databases (LanceDB, Azure Search)
- **Flexible Storage**: Options for local and cloud-based vector storage

## Repository Architecture

The codebase is organized into several key modules:

```
graphrag/
├── config/          # Configuration management and settings
├── index/           # Data indexing and processing pipeline
│   ├── workflows/   # Data processing workflows
│   ├── operations/  # Individual processing operations
│   └── graph/       # Graph construction and analysis
├── llm/             # LLM integration and management
│   ├── openai/      # OpenAI-specific implementations
│   └── base/        # Base LLM interfaces
├── model/           # Data models and schemas
├── prompt_tune/     # Prompt optimization and tuning tools
├── query/           # Query processing and retrieval
│   ├── global_search/  # Global search implementation
│   └── local_search/   # Local search implementation
└── vector_stores/   # Vector database integrations
```

## How to Use GraphRAG

### Installation and Setup
```bash
# Install using Poetry (recommended for development)
poetry install

# Or install from PyPI
pip install graphrag
```

### Quick Start Tutorial

#### Step 1: Initialize a GraphRAG Project
```bash
# Create a new directory for your project
mkdir my_graphrag_project
cd my_graphrag_project

# Initialize GraphRAG configuration
python -m graphrag.index --init
```

This creates:
- `settings.yaml` - Main configuration file
- `.env` - Environment variables (add your API keys here)
- `input/` - Directory for your source documents
- `prompts/` - Directory for customized prompts

#### Step 2: Configure Your API Keys
```bash
# Edit the .env file to add your OpenAI API key
echo "GRAPHRAG_API_KEY=your_openai_api_key_here" > .env
```

#### Step 3: Add Your Documents
```bash
# Place your text files in the input directory
cp your_documents.txt input/
```

#### Step 4: Run the Indexing Pipeline
```bash
# Process your documents to create the knowledge graph
python -m graphrag.index --root .
```

This process will:
- Extract entities and relationships from your documents
- Build a comprehensive knowledge graph
- Create hierarchical community summaries
- Generate vector embeddings for retrieval

#### Step 5: Query Your Knowledge Graph
```bash
# Perform global search across the entire knowledge graph
python -m graphrag.query \
  --root . \
  --method global \
  "What are the main themes and topics in the documents?"

# Perform local search focused on specific entities
python -m graphrag.query \
  --root . \
  --method local \
  "Tell me about relationships between key researchers"
```

### Command Line Interface

#### Indexing Commands
```bash
# Basic indexing
python -m graphrag.index --root /path/to/project

# Initialize new project
python -m graphrag.index --init --root /path/to/project

# Resume interrupted indexing
python -m graphrag.index --resume /path/to/output --root /path/to/project

# Dry run to validate configuration
python -m graphrag.index --dryrun --root /path/to/project

# Verbose output for debugging
python -m graphrag.index --verbose --root /path/to/project
```

#### Query Commands
```bash
# Global search with streaming output
python -m graphrag.query \
  --root /path/to/project \
  --method global \
  --streaming \
  "Your question here"

# Local search with specific community level
python -m graphrag.query \
  --root /path/to/project \
  --method local \
  --community_level 1 \
  "Your question here"

# Custom response format
python -m graphrag.query \
  --root /path/to/project \
  --method global \
  --response_type "List of 5 key points" \
  "Your question here"
```

#### Prompt Tuning Commands
```bash
# Auto-generate optimized prompts for your domain
python -m graphrag.prompt_tune \
  --config settings.yaml \
  --root /path/to/project \
  --domain "your domain expertise" \
  --output custom_prompts

# Use specific selection method for prompt generation
python -m graphrag.prompt_tune \
  --config settings.yaml \
  --selection-method auto \
  --limit 50 \
  --language "English"
```

## Use Cases and Applications

### 1. Enterprise Knowledge Management
- **Document Analysis**: Process large collections of corporate documents
- **Research Synthesis**: Combine insights from multiple research papers
- **Policy Analysis**: Understand complex regulatory frameworks

### 2. Academic Research
- **Literature Reviews**: Systematically analyze research literature
- **Cross-Domain Analysis**: Find connections between different research areas
- **Hypothesis Generation**: Discover potential research directions

### 3. Legal and Compliance
- **Contract Analysis**: Extract key terms and relationships from legal documents
- **Regulatory Compliance**: Understand complex compliance requirements
- **Case Law Research**: Analyze judicial decisions and precedents

### 4. Business Intelligence
- **Market Research**: Analyze market reports and competitor information
- **Customer Insights**: Process customer feedback and support tickets
- **Strategic Planning**: Synthesize business intelligence from various sources

## Configuration and Customization

GraphRAG offers extensive configuration options:

### Configuration Levels
1. **Minimal Configuration**: Basic setup with default parameters
2. **Workflow Configuration**: Custom processing workflows
3. **Advanced Configuration**: Fine-tuned parameters for specific use cases

## Configuration Examples and Templates

### Basic Configuration Template
```yaml
# settings.yaml - Minimal configuration for getting started

encoding_model: cl100k_base
skip_workflows: []

llm:
  api_key: ${GRAPHRAG_API_KEY}
  type: openai_chat
  model: gpt-4-turbo-preview
  model_supports_json: true
  max_tokens: 4000
  temperature: 0

parallelization:
  stagger: 0.3
  num_threads: 10  # Reduce for cost control

async_mode: threaded

embeddings:
  async_mode: threaded
  llm:
    api_key: ${GRAPHRAG_API_KEY}
    type: openai_embedding
    model: text-embedding-3-small

input:
  type: file
  file_type: text
  base_dir: "input"
  file_encoding: utf-8
  file_pattern: ".*\\.txt$"

cache:
  type: file
  base_dir: "cache"

storage:
  type: file
  base_dir: "output"

reporting:
  type: file
  base_dir: "reports"

entity_extraction:
  prompt: "prompts/entity_extraction.txt"
  entity_types: [person, organization, geo, event]
  max_gleanings: 1

summarize_descriptions:
  prompt: "prompts/summarize_descriptions.txt"
  max_length: 500

claim_extraction:
  enabled: true
  prompt: "prompts/claim_extraction.txt"
  description: "Any claims or facts that could be relevant to information discovery."
  max_gleanings: 1

community_report:
  prompt: "prompts/community_report.txt"
  max_length: 2000
  max_input_length: 8000

cluster_graph:
  max_cluster_size: 10

embed_graph:
  enabled: false  # Disable for cost savings

umap:
  enabled: true
```

### Azure OpenAI Configuration
```yaml
# For users with Azure OpenAI deployment

llm:
  api_key: ${GRAPHRAG_API_KEY}
  type: azure_openai_chat
  model: gpt-4
  api_base: https://your-instance.openai.azure.com
  api_version: 2024-02-15-preview
  deployment_name: your-gpt4-deployment

embeddings:
  llm:
    api_key: ${GRAPHRAG_API_KEY}
    type: azure_openai_embedding
    model: text-embedding-ada-002
    api_base: https://your-instance.openai.azure.com
    api_version: 2024-02-15-preview
    deployment_name: your-embedding-deployment
```

### High-Volume Processing Configuration
```yaml
# For processing large document collections

parallelization:
  stagger: 0.1
  num_threads: 50

llm:
  api_key: ${GRAPHRAG_API_KEY}
  type: openai_chat
  model: gpt-3.5-turbo  # Faster and cheaper for large volumes
  concurrent_requests: 25
  tokens_per_minute: 90000
  requests_per_minute: 3000
  max_retries: 5

embeddings:
  llm:
    api_key: ${GRAPHRAG_API_KEY}
    type: openai_embedding
    model: text-embedding-3-small
    concurrent_requests: 25
    tokens_per_minute: 1000000
    requests_per_minute: 3000

chunks:
  size: 300
  overlap: 100
  group_by_columns: [id]

input:
  type: file
  file_type: text
  base_dir: "input"
  file_pattern: ".*\\.(txt|md|pdf)$"
```

## Performance and Scalability

### Cost Considerations
⚠️ **Important**: GraphRAG indexing can be expensive as it makes extensive use of LLM APIs. Always:
- Start with small datasets to understand costs
- Monitor API usage and costs
- Consider using smaller models for initial testing

### Optimization Strategies
- **Batch Processing**: Process documents in batches to optimize API calls
- **Caching**: Leverage caching mechanisms to avoid redundant processing
- **Incremental Updates**: Update only changed documents rather than reprocessing everything

## Development and Contribution

### Development Setup
```bash
# Install development dependencies
poetry install

# Run tests
poetry run poe test

# Code formatting and linting
poetry run poe check
poetry run poe format
```

### Testing
- **Unit Tests**: `poetry run poe test_unit`
- **Integration Tests**: `poetry run poe test_integration`
- **Smoke Tests**: `poetry run poe test_smoke`

## Integration Examples

### Python API Usage

#### Basic Example
```python
import asyncio
from graphrag.query import GlobalSearch, LocalSearch
from graphrag.config import GraphRagConfig

async def main():
    # Load configuration
    config = GraphRagConfig.from_file("settings.yaml")
    
    # Initialize search engines
    global_search = GlobalSearch(config)
    local_search = LocalSearch(config)
    
    # Perform queries
    global_result = await global_search.asearch("What are the main themes?")
    local_result = await local_search.asearch("Tell me about entity X")
    
    print("Global Search Result:", global_result)
    print("Local Search Result:", local_result)

# Run the async function
asyncio.run(main())
```

#### Advanced Configuration Example
```python
from graphrag.index import create_pipeline_config
from graphrag.index.run import run_pipeline

# Create custom configuration
config = create_pipeline_config({
    "llm": {
        "api_key": "your_api_key",
        "type": "openai_chat",
        "model": "gpt-4"
    },
    "embeddings": {
        "api_key": "your_api_key", 
        "type": "openai_embedding",
        "model": "text-embedding-ada-002"
    },
    "input": {
        "type": "file",
        "base_dir": "./input"
    },
    "storage": {
        "type": "file",
        "base_dir": "./output"
    }
})

# Run the indexing pipeline
result = run_pipeline(config)
```

#### Working with Custom Workflows
```python
import pandas as pd
from graphrag.index import run_pipeline_with_config

# Prepare your data
documents_df = pd.DataFrame({
    "text": ["Document 1 content...", "Document 2 content..."],
    "id": ["doc1", "doc2"],
    "title": ["Title 1", "Title 2"]
})

# Run with custom workflow configuration
config_path = "custom_workflow.yaml"
result = run_pipeline_with_config(config_path, datasets={"input": documents_df})
```

## Practical Examples and Demos

### Demo 1: Basic Document Analysis

Let's walk through a complete example of analyzing a set of research documents:

```bash
# 1. Create project directory
mkdir research_analysis
cd research_analysis

# 2. Initialize GraphRAG
python -m graphrag.index --init

# 3. Configure API key
echo "GRAPHRAG_API_KEY=sk-your-openai-key" > .env

# 4. Add your research papers to input/
cp ~/Documents/research_papers/*.txt input/

# 5. Process the documents
python -m graphrag.index --root .

# 6. Query the knowledge graph
python -m graphrag.query \
  --root . \
  --method global \
  "What are the main research themes and how do they connect?"

python -m graphrag.query \
  --root . \
  --method local \
  "Who are the key researchers and what are their contributions?"
```

### Demo 2: Legal Document Analysis

```bash
# Set up for legal document processing
mkdir legal_analysis
cd legal_analysis
python -m graphrag.index --init

# Customize for legal domain
python -m graphrag.prompt_tune \
  --config settings.yaml \
  --domain "legal and regulatory documents" \
  --output legal_prompts \
  --language "English"

# Process legal documents
cp legal_documents/*.txt input/
python -m graphrag.index --root .

# Legal-specific queries
python -m graphrag.query \
  --root . \
  --method global \
  --response_type "Legal Brief Summary" \
  "What are the key legal precedents and regulatory requirements?"
```

### Demo 3: Business Intelligence Analysis

```python
# business_analysis.py - Custom script for business document processing

import asyncio
import os
from pathlib import Path
import pandas as pd
from graphrag.query import GlobalSearch, LocalSearch
from graphrag.config import GraphRagConfig

async def analyze_business_documents():
    """Analyze business documents and generate insights."""
    
    # Load configuration
    config = GraphRagConfig.from_file("settings.yaml")
    
    # Initialize search engines
    global_search = GlobalSearch(config)
    local_search = LocalSearch(config)
    
    # Business-specific queries
    queries = [
        "What are the main market trends and opportunities?",
        "Who are the key competitors and what are their strategies?", 
        "What are the biggest risks and challenges mentioned?",
        "What are the financial performance indicators?",
        "What strategic recommendations are provided?"
    ]
    
    results = {}
    for query in queries:
        print(f"Analyzing: {query}")
        result = await global_search.asearch(query)
        results[query] = result.response
        
    # Save results to report
    with open("business_analysis_report.md", "w") as f:
        f.write("# Business Intelligence Analysis Report\n\n")
        for query, result in results.items():
            f.write(f"## {query}\n\n{result}\n\n")
    
    print("Analysis complete. Report saved to business_analysis_report.md")

if __name__ == "__main__":
    asyncio.run(analyze_business_documents())
```

## Comparison with Traditional RAG

| Aspect | Traditional RAG | GraphRAG |
|--------|----------------|----------|
| Data Structure | Vector embeddings only | Knowledge graphs + vectors |
| Retrieval Method | Similarity search | Graph-based + similarity |
| Context Understanding | Limited | Rich relational context |
| Query Types | Simple similarity | Complex relational queries |
| Scalability | Good for simple queries | Better for complex reasoning |

## Best Practices

1. **Start Small**: Begin with a small dataset to understand the system
2. **Prompt Tuning**: Invest time in prompt optimization for your domain
3. **Monitor Costs**: Keep track of LLM API usage and costs
4. **Iterative Improvement**: Continuously refine your configuration
5. **Documentation**: Maintain clear documentation of your use case and configuration

## Troubleshooting and Support

### Common Issues
- **API Rate Limits**: Configure appropriate thread counts and delays
- **Memory Usage**: Monitor memory consumption with large datasets
- **Configuration Errors**: Validate configuration files before processing

### Resources
- [Official Documentation](https://microsoft.github.io/graphrag)
- [GitHub Discussions](https://github.com/microsoft/graphrag/discussions)
- [Microsoft Research Blog Post](https://www.microsoft.com/en-us/research/blog/graphrag-unlocking-llm-discovery-on-narrative-private-data/)
- [ArXiv Paper](https://arxiv.org/pdf/2404.16130)

## Responsible AI Considerations

GraphRAG includes comprehensive responsible AI guidelines covering:
- Intended use cases and limitations
- Performance evaluation metrics
- Bias and fairness considerations
- Privacy and security best practices

See [RAI_TRANSPARENCY.md](./RAI_TRANSPARENCY.md) for detailed information.

---

*This repository represents a research prototype and demonstration. While powerful, it should be used with appropriate understanding of its capabilities and limitations.*