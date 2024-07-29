# Nginx Deployment on Kubernetes

## Giới thiệu
Dự án này hướng dẫn cách triển khai một máy chủ Nginx trên Kubernetes với hỗ trợ SSL.

## Các bước triển khai

### Bước 1: Xây dựng Image Nginx và đẩy lên Docker Hub
Sử dụng các lệnh sau để xây dựng image từ Dockerfile và đẩy image lên Docker Hub:
```bash
    docker build -t viettu123/swarmtest:nginx -f Dockerfile .
    docker push viettu123/swarmtest:nginx
```

### Bước 2: Tạo các file SSL tự ký
Thực hiện các lệnh sau để tạo các file SSL tự ký:
```bash
    openssl req -nodes -newkey rsa:2048 -keyout tls.key -out ca.csr -subj "/CN=viettu.net"
    openssl x509 -req -sha256 -days 365 -in ca.csr -signkey tls.key -out tls.crt
```

### Bước 3: Tạo  Secret trong Kubernetes chứa các file SSL
```bash
    kubectl create secret tls secret-nginx-cert --cert=tls.crt --key=tls.key
```

### Bước 4: Triển khai lên Kubernetes
Triển khai file YAML bằng lệnh sau:
```bash
    kubectl apply -f nginx-deployment.yaml
```

## Kiểm tra triển khai
Truy cập vào dịch vụ thông qua địa chỉ IP của Node và các cổng tương ứng:

HTTP: http://<Node-IP>:31080
HTTPS: https://<Node-IP>:31443

## kết quả
![Mô tả ảnh](https://github.com/VIET-TU/images/blob/main/Screenshot%202024-07-29%20174638.jpg)
