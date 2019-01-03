# Automatic SOTA (state-of-the-art) extraction

Requires Python 3.6+.

```bash
pip install -r requirements.txt
```

## Getting the data

The data is kept in the [data](data/) directory. 

## JSON format description

The format consists of five primary data types: `Task`, `Dataset`, `Sota`, `SotaRow` and `Link`. 

A valid JSON file is a list of `Task` objects. You can see examples in the [data/tasks](https://github.com/atlasml/sota-extractor/tree/master/data/tasks) folder.

#### `Task`

A `Task` consists of the following fields:
- `task` - name of the task (string)
- `description` - short description of the task, in markdown (string)
- `subtasks` - a list of zero or more `Task` objects that are children to this task (list)
- `datasets` - a list of zero or more `Dataset` objects on which the tasks are evaluated (list)
- `source_link` - an optional `Link` object to the original source of the task

#### `Dataset`

A `Dataset` consists of the following fields:
- `dataset` - name of the dataset (string)
- `description` - a short description in markdown (string)
- `subdatasets` - zero or more children `Dataset` objects (e.g. dataset subsets or dataset partitions) (list)
- `dataset_links` - zero or more `Link` objects, representing the links to the dataset download page or any other relevant external pages (list)
- `dataset_citations"` - zero or more `Link` objects, representing the papers that are the primary citations for the dataset. 
- `sota` - the `Sota` object representing the state-of-the-art table on this dataset. 
 
#### `Link`

A `Link` object describes a URL, and has these two fields:
- `title` - title of the link, i.e. anchor text (string)
- `url` - target URL (string)

#### `Sota`

A `Sota` object represents one state-of-the-art table, with these fields:
- `metrics` - a list of metric names used to evaluate the methods (list of strings)
- `rows` a list of rows in the SOTA table, a list of `SotaRow` objects (list)

#### `SotaRow`

A `SotaRow` object represents one line of the SOTA table, it has these fields:
- `model_name` - Name of the model evaluated (string)
- `paper_title` - Primary paper's title (string)
- `paper_url` - Primary paper's URL (string)
- `paper_date` - Paper date of publishing, if available (string)
- `code_links` - a list of zero or more `Link` objects, with links to relevant code implementations (list)
- `model_links` - a list of zero or more `Link` objects, with links to relevant pretrained model files (list)
- `metrics` - a dictionary of values, where the keys are string from the parent `Sota.rows` list, and the values are the measured performance. (dictionary)

## Running the scrapers

### NLP-progress

[NLP-progress](https://github.com/sebastianruder/NLP-progress) is a hand-annotated collection of SOTA results from NLP tasks. 

The scraper [is part of the NLP-progress project](https://github.com/sebastianruder/NLP-progress/pull/186).

### EFF 

EFF has annotated a set of SOTA results on a small number of tasks, and produced this [great report](https://www.eff.org/ai/metrics).

Instructions are in the [scrapers/eff directory](scrapers/eff).

### SQuAD

The [Stanford Question Answering Dataset](https://rajpurkar.github.io/SQuAD-explorer/) is an active project for evaluating the question answering task using a hidden test set. 

To scrape the current content run:

```bash
python -m scrapers.squad
```


## Evaluating SOTA extraction

To evaluate the predictions for all tasks:

```bash
python -m extractor.eval_all
```

The most current report can be seen here: [eval_all_report.csv](eval_all_report.csv).

