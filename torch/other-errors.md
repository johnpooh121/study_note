# 여타 에러 해결 모음

## 디바이스

### device='cuda' 시 에러 발생

`torch.FloatTensor`에서 cuda device 구현이 안되있어서 문제가 발생할 수 있다고 함

`torch.tensor`에서 dtype=`torch.float32`로 해주면 됨