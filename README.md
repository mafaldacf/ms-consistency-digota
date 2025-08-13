# Prepare Environment

Create CA, and create and sign client certificate:
```zsh
certstrap init --common-name "ca"
certstrap request-cert --domain client.com
certstrap sign --CA "ca" client.com
```

# Deploy Infrastructure

Start mongodb and zookeeper:

```zsh
cd docker
docker-compose up -d
```

Start app:
```zsh
source .env
go run main.go
```

# Run Experiments

```zsh
cd _example
```

Experiment 1:
```zsh
go run product/new.go
export PRODUCT=85b18516-0261-4ba4-88cc-b4f48829c37f
go run product/get.go $PRODUCT

go run sku/new.go $PRODUCT
export SKU=55d0355d-1f99-4278-ab58-e223aba6a44d
go run sku/get.go $SKU

go run product/delete.go $PRODUCT

go run sku/update.go $SKU true
```

Experiment 2:

```zsh
go run product/new.go
export PRODUCT=c8d73b16-f1e3-4c6b-9d12-1f239204be61
go run product/get.go $PRODUCT

go run sku/new.go $PRODUCT
export SKU=849551ae-624b-4246-a79e-27fa442efdbb
go run sku/get.go $SKU

go run order/new.go $SKU
export ORDER=50441c48-2f79-464a-8907-c53ba01bc252
go run order/get.go $ORDER

go run sku/delete.go $SKU
go run order/return.go $ORDER
```

