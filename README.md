
1. Clone this example repo and branch

```
git clone -b s3usagefile https://github.com/infracost/example-terraform

cd example-terraform
```

2. Run a breakdown to ensure it works ok, total cost should show $12.6K total

```
infracost breakdown --path . --usage-file infracost-usage.yml
```

3. Generate a baseline json file to use for the what-if analysis

```
infracost breakdown --path . --usage-file infracost-usage.yml --format json --out-file infracost-base.json
```

4. Open infracost-usage.yml and change the aws_s3_bucket.data_bucket to move 100TB from standard to infrequent access:
- standard > storage_gb: 500000 to 400000
- uncomment standard_infrequent_access > storage_gb and set it to 100000

5. Run a cost diff with the above change, it should show the cost diff, where standard costs are going down but infrequent access goes up

```
infracost diff --path . --usage-file infracost-usage.yml --compare-to infracost-base.json
```

6. Open infracost-usage.yml and change the aws_s3_bucket.data_bucket to move 100TB from standard to glacier deep archive:
- standard > storage_gb: 400000 to 300000
- uncomment glacier_deep_archive > storage_gb and set it to 100000

7. Run a cost diff with the above change, where standard costs are going down further but glacier costs go up. Notice how much cheaper glacier is vs standard storage!

```
infracost diff --path . --usage-file infracost-usage.yml --compare-to infracost-base.json
```
