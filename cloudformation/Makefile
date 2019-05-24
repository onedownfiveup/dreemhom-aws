.PHONY: deploy

export DATABSE_PASSWORD ?= $(aws secretsmanager get-random-password \
--password-length 50 --exclude-characters "/'\"@,")

deploy/%:
	aws cloudformation deploy --template-file dreemhome-template.yml --stack-name dreemhome-api-$* \
    --parameter-overrides $$(cat $*.json | jq -r '.Parameters|to_entries[]|.key+"="+.value') \
    --capabilities CAPABILITY_NAMED_IAM

create-ecr-repo:
	aws cloudformation deploy --template-file ecr.yml --stack-name dreemhome-api-ecr \
		--parameter-overrides $$(cat dev.json | jq -r '.Parameters|to_entries[]|.key+"="+.value')


create-kms:
	aws cloudformation deploy --template-file kms.yml --stack-name dreemhome-api-kms \
		--parameter-overrides $$(cat dev.json | jq -r '.Parameters|to_entries[]|.key+"="+.value')

upload-stacks:
	for file in `ls *.yml`; do aws s3 cp --acl public-read $$file s3://dreemhome-cloudformation-templates/; done

