# Dataflow


## Feature

- k8s log dataflow  to GCS.
- Parse each pods log
- Pub/Sub to GCS (Compress GZip)
- timezone

## Preparation

```
pip install apache-beam
pip install apache-beam[gcp] ※don't use zsh
```

## Deploy & Execution

```
python dataflow.py \
  --project=<project_id> \
  --input_topic=projects/<project_id>/topics/<topic_name> \
  --output_path=gs://<bucket_name>/<backet_path> \
  --runner=DataflowRunner \
  --job_name=<job_name> \
  --window_size=10 \
  --temp_location=gs://<bucket_name>/<backet_path>/temp \
  --region=asia-northeast1  \
  --disk_size_gb=10 \
  --max_num_workers=2 \
  --machine_type=n1-standard-1\
  --zone=asia-northeast1-a
```

## Parse Examples

Change follows

```
extraction = (
    read_result | 'parser_1' >> beam.Filter(
        lambda log: log['labels']['k8s-pod/app'] == 'container_1') | 'extract parser_1' >> beam.Map(
        lambda x: payload(x)) | "into window_" >> GroupWindowsIntoBatches(window_size))
conversion = (
    read_result | 'parser_2' >> beam.Filter(
        lambda log: log['labels']['k8s-pod/app'] == 'container_2') | 'extract parser_2' >> beam.Map(
        lambda x: payload(x)) | "into window_2" >> GroupWindowsIntoBatches(window_size))
parse = (
    read_result | 'parser_3' >> beam.Filter(
        lambda log: log['labels']['k8s-pod/app'] == 'container_3') | 'extract parser_3' >> beam.Map(
        lambda x: payload(x)) | "into window_3" >> GroupWindowsIntoBatches(window_size))


```
