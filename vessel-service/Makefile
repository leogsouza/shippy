build:
		protoc -I. --go_out=plugins=micro:. \
			proto/vessel/vessel.proto
		docker build -t vessel-service .
run:
	docker run -d -p 50052:50051 \
		-e MICRO_SERVER_ADDRESS=:50051 \
		--name vessel_service vessel-service