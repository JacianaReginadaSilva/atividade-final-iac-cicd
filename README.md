# Infraestrutura AWS com Terraform e GitHub Actions

Provisionamento automatizado de infraestrutura na AWS (`us-east-1`) utilizando Terraform e GitHub Actions, com gerenciamento de estado via Artifacts para o ambiente AWS Academy Sandbox.

## Arquitetura Provisionada
- **Rede e Segurança:** VPC padrão e sub-redes da região `us-east-1`, com Security Groups restringindo o tráfego HTTP (porta 80) do ALB para a EC2 e o acesso MySQL (porta 3306) do RDS para a EC2.
- **Computação (EC2):** Instância `t3.micro` com Amazon Linux 2023 e servidor web Apache (`httpd`) configurado via `user_data`.
- **Balanceamento (ALB):** Application Load Balancer público na porta 80 encaminhando requisições para o Target Group da instância EC2.
- **Banco de Dados (RDS):** Instância MySQL `db.t3.micro` (`db-atividade3-mysql`) com senha passada via variável (`DB_PASSWORD`).

## Automação com GitHub Actions
- O workflow em `.github/workflows/deploy.yml` executa `terraform init`, `terraform validate`, `terraform plan`, `terraform apply` e `terraform output`.
- O arquivo `terraform.tfstate` é salvo e recuperado entre execuções usando `actions/upload-artifact` e `actions/download-artifact`.
- Credenciais AWS (`AWS_ACCESS_KEY_ID`, `AWS_SECRET_ACCESS_KEY`, `AWS_SESSION_TOKEN`) e `DB_PASSWORD` são armazenadas como Repository Secrets.

## Validação e Limpeza
- A aplicação é acessada pela URL pública do Application Load Balancer na porta 80.
- Após os testes e a gravação da demonstração, os recursos são removidos para manter o ambiente do laboratório limpo.
