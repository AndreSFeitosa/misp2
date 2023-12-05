# PHP build stage
FROM debian:bullseye-slim as php-build
# ... (mantenha as instruções existentes da etapa de construção do PHP)

# Python build stage
FROM debian:bullseye-slim as python-build
RUN apt-get update && apt-get install -y --no-install-recommends \
    # ... (mantenha as instalações existentes)
    python3-pip \
    # ... (e outras dependências necessárias)

# Aqui, assegure-se de que os arquivos .whl sejam criados e disponíveis em /wheels
RUN mkdir /wheels
# ... (Adicione comandos para instalar suas dependências do Python e criar arquivos .whl)
# Exemplo: RUN pip3 wheel numpy -w /wheels

# Final stage
FROM debian:bullseye-slim
ENV DEBIAN_FRONTEND noninteractive
# ... (mantenha as instalações de pacotes do sistema operacional existentes)

# Python Modules
COPY --from=python-build /wheels /wheels
# Aqui, estamos usando um comando explícito para instalar os arquivos .whl
RUN sh -c 'pip3 install --no-cache-dir /wheels/*.whl' && rm -rf /wheels

# PHP
# ... (mantenha as instalações existentes de extensões PHP)

# nginx
# ... (mantenha a configuração existente do nginx)

# Make a copy of the file store, so we can sync from it
# ... (mantenha os comandos existentes)

# Change Workdirectory
WORKDIR /var/www/MISP

# Set environment variable
ENV MINHA_VARIAVEL_DE_AMBIENTE=valor_padrao
