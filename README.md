# dataflow





# Preparation
pip install apache-beam
pip install apache-beam[gcp] ※don't use zsh

# Deploy & Execution
python dataflow.py \
  --project=lf-rd-analysis-dev \
  --input_topic=projects/rd-marshall-dev/topics/to-lf-rd-analysis-dev \
  --output_path=gs://lf-rd-analysis-dev_embulk_output/rdlogs/marshall/gcp/ \
  --runner=DataflowRunner \
  --job_name=rd-marshall-dev-log \
  --window_size=10 \
  --temp_location=gs://lf-rd-analysis-dev_embulk_output/gcp/temp \
  --region=asia-northeast1  \
  --disk_size_gb=20 \
  --max_num_workers=2 \
  --machine_type=n1-standard-1\
  --zone=asia-northeast1-a
