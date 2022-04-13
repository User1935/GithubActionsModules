[INFO] Initializing environment for https://github.com/pre-commit/pre-commit-hooks.git.
[INFO] Initializing environment for https://github.com/antonbabenko/pre-commit-terraform.
[INFO] Installing environment for https://github.com/pre-commit/pre-commit-hooks.git.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
[INFO] Installing environment for https://github.com/antonbabenko/pre-commit-terraform.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
mixed line ending........................................................Passed
trim trailing whitespace.................................................Passed
check yaml...............................................................Passed
check json...............................................................Passed
check for merge conflicts................................................Passed
fix double quoted strings............................(no files to check)Skipped
fix end of files.........................................................Failed
- hook id: end-of-file-fixer
- exit code: 1
- files were modified by this hook

Fixing .gitignore

Terraform fmt............................................................Passed
Terraform docs...........................................................Passed
terrascan................................................................Failed
- hook id: terrascan
- exit code: 4

Scan Summary -

	File/Folder         :	/github/workspace
	IaC Type            :	terraform
	Scanned At          :	2022-03-22 14:52:21.030234988 +0000 UTC
	Policies Validated  :	0
	Violated Policies   :	0
	Low                 :	0
	Medium              :	0
	High                :	0

Terraform validate with tflint...........................................Passed
Terraform validate with tfsec............................................Passed
Checkov..................................................................Failed
- hook id: checkov
- exit code: 1

_               _              
   ___| |__   ___  ___| | _______   __
  / __| '_ \ / _ \/ __| |/ / _ \ \ / /
 | (__| | | |  __/ (__|   < (_) \ V / 
  \___|_| |_|\___|\___|_|\_\___/ \_/  
                                      
By bridgecrew.io | version: 2.0.654 
Update available 2.0.654 -> 2.0.981
Run pip3 install -U checkov to update 


dockerfile scan results:

Passed checks: 5, Failed checks: 4, Skipped checks: 0

Check: CKV_DOCKER_5: "Ensure update instructions are not use alone in the Dockerfile"
	PASSED for resource: /actions/pre-commit/Dockerfile.
	File: /actions/pre-commit/Dockerfile:1-17
	Guide: https://docs.bridgecrew.io/docs/ensure-update-instructions-are-not-used-alone-in-the-dockerfile

Check: CKV_DOCKER_3: "Ensure that a user for the container has been created"
	PASSED for resource: /actions/pre-commit/Dockerfile.USER
	File: /actions/pre-commit/Dockerfile:16-16
	Guide: https://docs.bridgecrew.io/docs/ensure-that-a-user-for-the-container-has-been-created

Check: CKV_DOCKER_2: "Ensure that HEALTHCHECK instructions have been added to container images"
	PASSED for resource: /actions/pre-commit/Dockerfile.HEALTHCHECK
	File: /actions/pre-commit/Dockerfile:11-11
	Guide: https://docs.bridgecrew.io/docs/ensure-that-healthcheck-instructions-have-been-added-to-container-images

Check: CKV_DOCKER_7: "Ensure the base image uses a non latest version tag"
	PASSED for resource: /actions/pre-commitv2/Dockerfile.
	File: /actions/pre-commitv2/Dockerfile:1-181
	Guide: https://docs.bridgecrew.io/docs/ensure-the-base-image-uses-a-non-latest-version-tag

Check: CKV_DOCKER_5: "Ensure update instructions are not use alone in the Dockerfile"
	PASSED for resource: /actions/pre-commitv2/Dockerfile.
	File: /actions/pre-commitv2/Dockerfile:1-181
	Guide: https://docs.bridgecrew.io/docs/ensure-update-instructions-are-not-used-alone-in-the-dockerfile

Check: CKV_DOCKER_7: "Ensure the base image uses a non latest version tag"
	FAILED for resource: /actions/pre-commit/Dockerfile.FROM
	File: /actions/pre-commit/Dockerfile:1-1
	Guide: https://docs.bridgecrew.io/docs/ensure-the-base-image-uses-a-non-latest-version-tag

		1 | FROM ghcr.io/antonbabenko/pre-commit-terraform:latest


Check: CKV_DOCKER_8: "Ensure the last USER is not root"
	FAILED for resource: /actions/pre-commit/Dockerfile.USER
	File: /actions/pre-commit/Dockerfile:16-16
	Guide: https://docs.bridgecrew.io/docs/ensure-the-last-user-is-not-root

		16 | USER root


Check: CKV_DOCKER_3: "Ensure that a user for the container has been created"
	FAILED for resource: /actions/pre-commitv2/Dockerfile.
	File: /actions/pre-commitv2/Dockerfile:1-181
	Guide: https://docs.bridgecrew.io/docs/ensure-that-a-user-for-the-container-has-been-created

		1   | ARG TAG=3.10.1-alpine3.15
		2   | FROM python:${TAG} as builder
		3   | 
		4   | WORKDIR /bin_dir
		5   | 
		6   | RUN apk add --no-cache \
		7   |     # Builder deps
		8   |     curl=~7 \
		9   |     unzip=~6 && \
		10  |     # Upgrade pip for be able get latest Checkov
		11  |     python3 -m pip install --no-cache-dir --upgrade pip
		12  | 
		13  | ARG PRE_COMMIT_VERSION=${PRE_COMMIT_VERSION:-latest}
		14  | ARG TERRAFORM_VERSION=${TERRAFORM_VERSION:-latest}
		15  | 
		16  | # Install pre-commit
		17  | RUN [ ${PRE_COMMIT_VERSION} = "latest" ] && pip3 install --no-cache-dir pre-commit \
		18  |     || pip3 install --no-cache-dir pre-commit==${PRE_COMMIT_VERSION}
		19  | 
		20  | # Install terraform because pre-commit needs it
		21  | RUN if [ "${TERRAFORM_VERSION}" = "latest" ]; then \
		22  |         TERRAFORM_VERSION="$(curl -s https://api.github.com/repos/hashicorp/terraform/releases/latest | grep tag_name | grep -o -E -m 1 "[0-9.]+")" \
		23  |     ; fi && \
		24  |     curl -L "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip" > terraform.zip && \
		25  |     unzip terraform.zip terraform && rm terraform.zip
		26  | 
		27  | #
		28  | # Install tools
		29  | #
		30  | ARG CHECKOV_VERSION=${CHECKOV_VERSION:-false}
		31  | ARG INFRACOST_VERSION=${INFRACOST_VERSION:-false}
		32  | ARG TERRAFORM_DOCS_VERSION=${TERRAFORM_DOCS_VERSION:-false}
		33  | ARG TERRAGRUNT_VERSION=${TERRAGRUNT_VERSION:-false}
		34  | ARG TERRASCAN_VERSION=${TERRASCAN_VERSION:-false}
		35  | ARG TFLINT_VERSION=${TFLINT_VERSION:-false}
		36  | ARG TFSEC_VERSION=${TFSEC_VERSION:-false}
		37  | 
		38  | 
		39  | # Tricky thing to install all tools by set only one arg.
		40  | # In RUN command below used `. /.env` <- this is sourcing vars that
		41  | # specified in step below
		42  | ARG INSTALL_ALL=${INSTALL_ALL:-false}
		43  | RUN if [ "$INSTALL_ALL" != "false" ]; then \
		44  |         echo "export CHECKOV_VERSION=latest" >> /.env && \
		45  |         echo "export INFRACOST_VERSION=latest" >> /.env && \
		46  |         echo "export TERRAFORM_DOCS_VERSION=latest" >> /.env && \
		47  |         echo "export TERRAGRUNT_VERSION=latest" >> /.env && \
		48  |         echo "export TERRASCAN_VERSION=latest" >> /.env && \
		49  |         echo "export TFLINT_VERSION=latest" >> /.env && \
		50  |         echo "export TFSEC_VERSION=latest" >> /.env \
		51  |     ; else \
		52  |         touch /.env \
		53  |     ; fi
		54  | 
		55  | 
		56  | # Checkov
		57  | RUN . /.env && \
		58  |     if [ "$CHECKOV_VERSION" != "false" ]; then \
		59  |     ( \
		60  |         apk add --no-cache gcc=~10 libffi-dev=~3 musl-dev=~1; \
		61  |         [ "$CHECKOV_VERSION" = "latest" ] && pip3 install --no-cache-dir checkov \
		62  |         || pip3 install --no-cache-dir checkov==${CHECKOV_VERSION}; \
		63  |         apk del gcc libffi-dev musl-dev \
		64  |     ) \
		65  |     ; fi
		66  | 
		67  | # infracost
		68  | RUN . /.env && \
		69  |     if [ "$INFRACOST_VERSION" != "false" ]; then \
		70  |     ( \
		71  |         INFRACOST_RELEASES="https://api.github.com/repos/infracost/infracost/releases" && \
		72  |         [ "$INFRACOST_VERSION" = "latest" ] && curl -L "$(curl -s ${INFRACOST_RELEASES}/latest | grep -o -E -m 1 "https://.+?-linux-amd64.tar.gz")" > infracost.tgz \
		73  |         || curl -L "$(curl -s ${INFRACOST_RELEASES} | grep -o -E "https://.+?v${INFRACOST_VERSION}/infracost-linux-amd64.tar.gz")" > infracost.tgz \
		74  |     ) && tar -xzf infracost.tgz && rm infracost.tgz && mv infracost-linux-amd64 infracost \
		75  |     ; fi
		76  | 
		77  | # Terraform docs
		78  | RUN . /.env && \
		79  |     if [ "$TERRAFORM_DOCS_VERSION" != "false" ]; then \
		80  |     ( \
		81  |         TERRAFORM_DOCS_RELEASES="https://api.github.com/repos/terraform-docs/terraform-docs/releases" && \
		82  |         [ "$TERRAFORM_DOCS_VERSION" = "latest" ] && curl -L "$(curl -s ${TERRAFORM_DOCS_RELEASES}/latest | grep -o -E -m 1 "https://.+?-linux-amd64.tar.gz")" > terraform-docs.tgz \
		83  |         || curl -L "$(curl -s ${TERRAFORM_DOCS_RELEASES} | grep -o -E "https://.+?v${TERRAFORM_DOCS_VERSION}-linux-amd64.tar.gz")" > terraform-docs.tgz \
		84  |     ) && tar -xzf terraform-docs.tgz terraform-docs && rm terraform-docs.tgz && chmod +x terraform-docs \
		85  |     ; fi
		86  | 
		87  | # Terragrunt
		88  | RUN . /.env \
		89  |     && if [ "$TERRAGRUNT_VERSION" != "false" ]; then \
		90  |     ( \
		91  |         TERRAGRUNT_RELEASES="https://api.github.com/repos/gruntwork-io/terragrunt/releases" && \
		92  |         [ "$TERRAGRUNT_VERSION" = "latest" ] && curl -L "$(curl -s ${TERRAGRUNT_RELEASES}/latest | grep -o -E -m 1 "https://.+?/terragrunt_linux_amd64")" > terragrunt \
		93  |         || curl -L "$(curl -s ${TERRAGRUNT_RELEASES} | grep -o -E -m 1 "https://.+?v${TERRAGRUNT_VERSION}/terragrunt_linux_amd64")" > terragrunt \
		94  |     ) && chmod +x terragrunt \
		95  |     ; fi
		96  | 
		97  | 
		98  | # Terrascan
		99  | RUN . /.env && \
		100 |     if [ "$TERRASCAN_VERSION" != "false" ]; then \
		101 |     ( \
		102 |         TERRASCAN_RELEASES="https://api.github.com/repos/accurics/terrascan/releases" && \
		103 |         [ "$TERRASCAN_VERSION" = "latest" ] && curl -L "$(curl -s ${TERRASCAN_RELEASES}/latest | grep -o -E -m 1 "https://.+?_Linux_x86_64.tar.gz")" > terrascan.tar.gz \
		104 |         || curl -L "$(curl -s ${TERRASCAN_RELEASES} | grep -o -E "https://.+?${TERRASCAN_VERSION}_Linux_x86_64.tar.gz")" > terrascan.tar.gz \
		105 |     ) && tar -xzf terrascan.tar.gz terrascan && rm terrascan.tar.gz && \
		106 |     ./terrascan init \
		107 |     ; fi
		108 | 
		109 | # TFLint
		110 | RUN . /.env && \
		111 |     if [ "$TFLINT_VERSION" != "false" ]; then \
		112 |     ( \
		113 |         TFLINT_RELEASES="https://api.github.com/repos/terraform-linters/tflint/releases" && \
		114 |         [ "$TFLINT_VERSION" = "latest" ] && curl -L "$(curl -s ${TFLINT_RELEASES}/latest | grep -o -E -m 1 "https://.+?_linux_amd64.zip")" > tflint.zip \
		115 |         || curl -L "$(curl -s ${TFLINT_RELEASES} | grep -o -E "https://.+?/v${TFLINT_VERSION}/tflint_linux_amd64.zip")" > tflint.zip \
		116 |     ) && unzip tflint.zip && rm tflint.zip \
		117 |     ; fi
		118 | 
		119 | # TFSec
		120 | RUN . /.env && \
		121 |     if [ "$TFSEC_VERSION" != "false" ]; then \
		122 |     ( \
		123 |         TFSEC_RELEASES="https://api.github.com/repos/aquasecurity/tfsec/releases" && \
		124 |         [ "$TFSEC_VERSION" = "latest" ] && curl -L "$(curl -s ${TFSEC_RELEASES}/latest | grep -o -E -m 1 "https://.+?/tfsec-linux-amd64")" > tfsec \
		125 |         || curl -L "$(curl -s ${TFSEC_RELEASES} | grep -o -E -m 1 "https://.+?v${TFSEC_VERSION}/tfsec-linux-amd64")" > tfsec \
		126 |     ) && chmod +x tfsec \
		127 |     ; fi
		128 | 
		129 | # Checking binaries versions and write it to debug file
		130 | RUN . /.env && \
		131 |     F=tools_versions_info && \
		132 |     pre-commit --version >> $F && \
		133 |     ./terraform --version | head -n 1 >> $F && \
		134 |     (if [ "$CHECKOV_VERSION"        != "false" ]; then echo "checkov $(checkov --version)" >> $F;     else echo "checkov SKIPPED" >> $F        ; fi) && \
		135 |     (if [ "$INFRACOST_VERSION"      != "false" ]; then echo "$(./infracost --version)" >> $F;         else echo "infracost SKIPPED" >> $F      ; fi) && \
		136 |     (if [ "$TERRAFORM_DOCS_VERSION" != "false" ]; then ./terraform-docs --version >> $F;              else echo "terraform-docs SKIPPED" >> $F ; fi) && \
		137 |     (if [ "$TERRAGRUNT_VERSION"     != "false" ]; then ./terragrunt --version >> $F;                  else echo "terragrunt SKIPPED" >> $F     ; fi) && \
		138 |     (if [ "$TERRASCAN_VERSION"      != "false" ]; then echo "terrascan $(./terrascan version)" >> $F; else echo "terrascan SKIPPED" >> $F      ; fi) && \
		139 |     (if [ "$TFLINT_VERSION"         != "false" ]; then ./tflint --version >> $F;                      else echo "tflint SKIPPED" >> $F         ; fi) && \
		140 |     (if [ "$TFSEC_VERSION"          != "false" ]; then echo "tfsec $(./tfsec --version)" >> $F;       else echo "tfsec SKIPPED" >> $F          ; fi) && \
		141 |     echo -e "\n\n" && cat $F && echo -e "\n\n"
		142 | 
		143 | 
		144 | 
		145 | FROM python:${TAG}
		146 | 
		147 | RUN apk add --no-cache \
		148 |     # pre-commit deps
		149 |     git=~2 \
		150 |     # All hooks deps
		151 |     bash=~5
		152 | 
		153 | # Copy tools
		154 | COPY --from=builder \
		155 |     # Needed for all hooks
		156 |     /usr/local/bin/pre-commit \
		157 |     # Hooks and terraform binaries
		158 |     /bin_dir/ \
		159 |     /usr/local/bin/checkov* \
		160 |         /usr/bin/
		161 | # Copy pre-commit packages
		162 | COPY --from=builder /usr/local/lib/python3.10/site-packages/ /usr/local/lib/python3.10/site-packages/
		163 | # Copy terrascan policies
		164 | COPY --from=builder /root/ /root/
		165 | 
		166 | # Install hooks extra deps
		167 | RUN if [ "$(grep -o '^terraform-docs SKIPPED$' /usr/bin/tools_versions_info)" = "" ]; then \
		168 |         apk add --no-cache perl=~5 \
		169 |     ; fi && \
		170 |     if [ "$(grep -o '^infracost SKIPPED$' /usr/bin/tools_versions_info)" = "" ]; then \
		171 |         apk add --no-cache jq=~1 \
		172 |     ; fi
		173 | 
		174 | ENV PRE_COMMIT_COLOR=${PRE_COMMIT_COLOR:-always}
		175 | 
		176 | ENV INFRACOST_API_KEY=${INFRACOST_API_KEY:-}
		177 | ENV INFRACOST_SKIP_UPDATE_CHECK=${INFRACOST_SKIP_UPDATE_CHECK:-false}
		178 | 
		179 | COPY ["src", "/src/"]
		180 | RUN ["chmod", "+x", "/src/main.sh"]
		181 | ENTRYPOINT ["/src/main.sh"]


Check: CKV_DOCKER_2: "Ensure that HEALTHCHECK instructions have been added to container images"
	FAILED for resource: /actions/pre-commitv2/Dockerfile.
	File: /actions/pre-commitv2/Dockerfile:1-181
	Guide: https://docs.bridgecrew.io/docs/ensure-that-healthcheck-instructions-have-been-added-to-container-images

		1   | ARG TAG=3.10.1-alpine3.15
		2   | FROM python:${TAG} as builder
		3   | 
		4   | WORKDIR /bin_dir
		5   | 
		6   | RUN apk add --no-cache \
		7   |     # Builder deps
		8   |     curl=~7 \
		9   |     unzip=~6 && \
		10  |     # Upgrade pip for be able get latest Checkov
		11  |     python3 -m pip install --no-cache-dir --upgrade pip
		12  | 
		13  | ARG PRE_COMMIT_VERSION=${PRE_COMMIT_VERSION:-latest}
		14  | ARG TERRAFORM_VERSION=${TERRAFORM_VERSION:-latest}
		15  | 
		16  | # Install pre-commit
		17  | RUN [ ${PRE_COMMIT_VERSION} = "latest" ] && pip3 install --no-cache-dir pre-commit \
		18  |     || pip3 install --no-cache-dir pre-commit==${PRE_COMMIT_VERSION}
		19  | 
		20  | # Install terraform because pre-commit needs it
		21  | RUN if [ "${TERRAFORM_VERSION}" = "latest" ]; then \
		22  |         TERRAFORM_VERSION="$(curl -s https://api.github.com/repos/hashicorp/terraform/releases/latest | grep tag_name | grep -o -E -m 1 "[0-9.]+")" \
		23  |     ; fi && \
		24  |     curl -L "https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip" > terraform.zip && \
		25  |     unzip terraform.zip terraform && rm terraform.zip
		26  | 
		27  | #
		28  | # Install tools
		29  | #
		30  | ARG CHECKOV_VERSION=${CHECKOV_VERSION:-false}
		31  | ARG INFRACOST_VERSION=${INFRACOST_VERSION:-false}
		32  | ARG TERRAFORM_DOCS_VERSION=${TERRAFORM_DOCS_VERSION:-false}
		33  | ARG TERRAGRUNT_VERSION=${TERRAGRUNT_VERSION:-false}
		34  | ARG TERRASCAN_VERSION=${TERRASCAN_VERSION:-false}
		35  | ARG TFLINT_VERSION=${TFLINT_VERSION:-false}
		36  | ARG TFSEC_VERSION=${TFSEC_VERSION:-false}
		37  | 
		38  | 
		39  | # Tricky thing to install all tools by set only one arg.
		40  | # In RUN command below used `. /.env` <- this is sourcing vars that
		41  | # specified in step below
		42  | ARG INSTALL_ALL=${INSTALL_ALL:-false}
		43  | RUN if [ "$INSTALL_ALL" != "false" ]; then \
		44  |         echo "export CHECKOV_VERSION=latest" >> /.env && \
		45  |         echo "export INFRACOST_VERSION=latest" >> /.env && \
		46  |         echo "export TERRAFORM_DOCS_VERSION=latest" >> /.env && \
		47  |         echo "export TERRAGRUNT_VERSION=latest" >> /.env && \
		48  |         echo "export TERRASCAN_VERSION=latest" >> /.env && \
		49  |         echo "export TFLINT_VERSION=latest" >> /.env && \
		50  |         echo "export TFSEC_VERSION=latest" >> /.env \
		51  |     ; else \
		52  |         touch /.env \
		53  |     ; fi
		54  | 
		55  | 
		56  | # Checkov
		57  | RUN . /.env && \
		58  |     if [ "$CHECKOV_VERSION" != "false" ]; then \
		59  |     ( \
		60  |         apk add --no-cache gcc=~10 libffi-dev=~3 musl-dev=~1; \
		61  |         [ "$CHECKOV_VERSION" = "latest" ] && pip3 install --no-cache-dir checkov \
		62  |         || pip3 install --no-cache-dir checkov==${CHECKOV_VERSION}; \
		63  |         apk del gcc libffi-dev musl-dev \
		64  |     ) \
		65  |     ; fi
		66  | 
		67  | # infracost
		68  | RUN . /.env && \
		69  |     if [ "$INFRACOST_VERSION" != "false" ]; then \
		70  |     ( \
		71  |         INFRACOST_RELEASES="https://api.github.com/repos/infracost/infracost/releases" && \
		72  |         [ "$INFRACOST_VERSION" = "latest" ] && curl -L "$(curl -s ${INFRACOST_RELEASES}/latest | grep -o -E -m 1 "https://.+?-linux-amd64.tar.gz")" > infracost.tgz \
		73  |         || curl -L "$(curl -s ${INFRACOST_RELEASES} | grep -o -E "https://.+?v${INFRACOST_VERSION}/infracost-linux-amd64.tar.gz")" > infracost.tgz \
		74  |     ) && tar -xzf infracost.tgz && rm infracost.tgz && mv infracost-linux-amd64 infracost \
		75  |     ; fi
		76  | 
		77  | # Terraform docs
		78  | RUN . /.env && \
		79  |     if [ "$TERRAFORM_DOCS_VERSION" != "false" ]; then \
		80  |     ( \
		81  |         TERRAFORM_DOCS_RELEASES="https://api.github.com/repos/terraform-docs/terraform-docs/releases" && \
		82  |         [ "$TERRAFORM_DOCS_VERSION" = "latest" ] && curl -L "$(curl -s ${TERRAFORM_DOCS_RELEASES}/latest | grep -o -E -m 1 "https://.+?-linux-amd64.tar.gz")" > terraform-docs.tgz \
		83  |         || curl -L "$(curl -s ${TERRAFORM_DOCS_RELEASES} | grep -o -E "https://.+?v${TERRAFORM_DOCS_VERSION}-linux-amd64.tar.gz")" > terraform-docs.tgz \
		84  |     ) && tar -xzf terraform-docs.tgz terraform-docs && rm terraform-docs.tgz && chmod +x terraform-docs \
		85  |     ; fi
		86  | 
		87  | # Terragrunt
		88  | RUN . /.env \
		89  |     && if [ "$TERRAGRUNT_VERSION" != "false" ]; then \
		90  |     ( \
		91  |         TERRAGRUNT_RELEASES="https://api.github.com/repos/gruntwork-io/terragrunt/releases" && \
		92  |         [ "$TERRAGRUNT_VERSION" = "latest" ] && curl -L "$(curl -s ${TERRAGRUNT_RELEASES}/latest | grep -o -E -m 1 "https://.+?/terragrunt_linux_amd64")" > terragrunt \
		93  |         || curl -L "$(curl -s ${TERRAGRUNT_RELEASES} | grep -o -E -m 1 "https://.+?v${TERRAGRUNT_VERSION}/terragrunt_linux_amd64")" > terragrunt \
		94  |     ) && chmod +x terragrunt \
		95  |     ; fi
		96  | 
		97  | 
		98  | # Terrascan
		99  | RUN . /.env && \
		100 |     if [ "$TERRASCAN_VERSION" != "false" ]; then \
		101 |     ( \
		102 |         TERRASCAN_RELEASES="https://api.github.com/repos/accurics/terrascan/releases" && \
		103 |         [ "$TERRASCAN_VERSION" = "latest" ] && curl -L "$(curl -s ${TERRASCAN_RELEASES}/latest | grep -o -E -m 1 "https://.+?_Linux_x86_64.tar.gz")" > terrascan.tar.gz \
		104 |         || curl -L "$(curl -s ${TERRASCAN_RELEASES} | grep -o -E "https://.+?${TERRASCAN_VERSION}_Linux_x86_64.tar.gz")" > terrascan.tar.gz \
		105 |     ) && tar -xzf terrascan.tar.gz terrascan && rm terrascan.tar.gz && \
		106 |     ./terrascan init \
		107 |     ; fi
		108 | 
		109 | # TFLint
		110 | RUN . /.env && \
		111 |     if [ "$TFLINT_VERSION" != "false" ]; then \
		112 |     ( \
		113 |         TFLINT_RELEASES="https://api.github.com/repos/terraform-linters/tflint/releases" && \
		114 |         [ "$TFLINT_VERSION" = "latest" ] && curl -L "$(curl -s ${TFLINT_RELEASES}/latest | grep -o -E -m 1 "https://.+?_linux_amd64.zip")" > tflint.zip \
		115 |         || curl -L "$(curl -s ${TFLINT_RELEASES} | grep -o -E "https://.+?/v${TFLINT_VERSION}/tflint_linux_amd64.zip")" > tflint.zip \
		116 |     ) && unzip tflint.zip && rm tflint.zip \
		117 |     ; fi
		118 | 
		119 | # TFSec
		120 | RUN . /.env && \
		121 |     if [ "$TFSEC_VERSION" != "false" ]; then \
		122 |     ( \
		123 |         TFSEC_RELEASES="https://api.github.com/repos/aquasecurity/tfsec/releases" && \
		124 |         [ "$TFSEC_VERSION" = "latest" ] && curl -L "$(curl -s ${TFSEC_RELEASES}/latest | grep -o -E -m 1 "https://.+?/tfsec-linux-amd64")" > tfsec \
		125 |         || curl -L "$(curl -s ${TFSEC_RELEASES} | grep -o -E -m 1 "https://.+?v${TFSEC_VERSION}/tfsec-linux-amd64")" > tfsec \
		126 |     ) && chmod +x tfsec \
		127 |     ; fi
		128 | 
		129 | # Checking binaries versions and write it to debug file
		130 | RUN . /.env && \
		131 |     F=tools_versions_info && \
		132 |     pre-commit --version >> $F && \
		133 |     ./terraform --version | head -n 1 >> $F && \
		134 |     (if [ "$CHECKOV_VERSION"        != "false" ]; then echo "checkov $(checkov --version)" >> $F;     else echo "checkov SKIPPED" >> $F        ; fi) && \
		135 |     (if [ "$INFRACOST_VERSION"      != "false" ]; then echo "$(./infracost --version)" >> $F;         else echo "infracost SKIPPED" >> $F      ; fi) && \
		136 |     (if [ "$TERRAFORM_DOCS_VERSION" != "false" ]; then ./terraform-docs --version >> $F;              else echo "terraform-docs SKIPPED" >> $F ; fi) && \
		137 |     (if [ "$TERRAGRUNT_VERSION"     != "false" ]; then ./terragrunt --version >> $F;                  else echo "terragrunt SKIPPED" >> $F     ; fi) && \
		138 |     (if [ "$TERRASCAN_VERSION"      != "false" ]; then echo "terrascan $(./terrascan version)" >> $F; else echo "terrascan SKIPPED" >> $F      ; fi) && \
		139 |     (if [ "$TFLINT_VERSION"         != "false" ]; then ./tflint --version >> $F;                      else echo "tflint SKIPPED" >> $F         ; fi) && \
		140 |     (if [ "$TFSEC_VERSION"          != "false" ]; then echo "tfsec $(./tfsec --version)" >> $F;       else echo "tfsec SKIPPED" >> $F          ; fi) && \
		141 |     echo -e "\n\n" && cat $F && echo -e "\n\n"
		142 | 
		143 | 
		144 | 
		145 | FROM python:${TAG}
		146 | 
		147 | RUN apk add --no-cache \
		148 |     # pre-commit deps
		149 |     git=~2 \
		150 |     # All hooks deps
		151 |     bash=~5
		152 | 
		153 | # Copy tools
		154 | COPY --from=builder \
		155 |     # Needed for all hooks
		156 |     /usr/local/bin/pre-commit \
		157 |     # Hooks and terraform binaries
		158 |     /bin_dir/ \
		159 |     /usr/local/bin/checkov* \
		160 |         /usr/bin/
		161 | # Copy pre-commit packages
		162 | COPY --from=builder /usr/local/lib/python3.10/site-packages/ /usr/local/lib/python3.10/site-packages/
		163 | # Copy terrascan policies
		164 | COPY --from=builder /root/ /root/
		165 | 
		166 | # Install hooks extra deps
		167 | RUN if [ "$(grep -o '^terraform-docs SKIPPED$' /usr/bin/tools_versions_info)" = "" ]; then \
		168 |         apk add --no-cache perl=~5 \
		169 |     ; fi && \
		170 |     if [ "$(grep -o '^infracost SKIPPED$' /usr/bin/tools_versions_info)" = "" ]; then \
		171 |         apk add --no-cache jq=~1 \
		172 |     ; fi
		173 | 
		174 | ENV PRE_COMMIT_COLOR=${PRE_COMMIT_COLOR:-always}
		175 | 
		176 | ENV INFRACOST_API_KEY=${INFRACOST_API_KEY:-}
		177 | ENV INFRACOST_SKIP_UPDATE_CHECK=${INFRACOST_SKIP_UPDATE_CHECK:-false}
		178 | 
		179 | COPY ["src", "/src/"]
		180 | RUN ["chmod", "+x", "/src/main.sh"]
		181 | ENTRYPOINT ["/src/main.sh"]




