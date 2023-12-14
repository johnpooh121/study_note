# Remarks

2023.12.15

np.cov는 sample variance일 경우를 고려해서 S=np.matmul(X,X.T)/(n-1) 로 계산된다!