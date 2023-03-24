clc;clear
%% 输入O-C判断矩阵
A = [1 2;
    1/2 1
    ];
[n,n]=size(A);

%% 算数平均法求权重
Sum_A = sum(A);
SUM_A = repmat(Sum_A, n, 1);
Stand_A = A ./ SUM_A;

disp('算数平均法求权重的结果为：');
disp(sum(Stand_A, 2) ./ n);

%% 几何平均法求权重
Product_A = prod(A, 2);
Product_n_A = Product_A .^ (1/n);

disp('几何平均法求权重的结果为：');
disp(Product_n_A ./ sum(Product_n_A));

%% 特征值法求权重
[V, D] = eig(A);
Max_eig = max(max(D));
[r, c] = find(D == Max_eig, 1);

disp('特征值法求权重的结果为：');
disp(V(:, c) ./ sum(V(:, c)));
disp('最大特征值 = ');
disp(Max_eig);

%% 计算一致性比例CR
CI = (Max_eig - n) / (n - 1);
RI = [0 0.0001 0.52 0.89 1.12 1.26 1.36 1.41 1.46 1.49 1.52 1.54 1.56 1.58 1.59];
CR = CI / RI(n);

disp('一致性指标CI = ');disp(CI);
disp('一致性比例CR = ');disp(CR);
if CR < 0.10
    disp('因为CR < 0.10，因此该判断矩阵的一致性可以接受！');
else
    disp('注意：CR > 0.10，因此需要对判断矩阵进行微调！');
end

%% 计算得分
result_C = [0.1062;
    0.6333;
    0.2605
    ];
result_P = [0.75 0 0.6667;
    0.25 0.1667 0.3333;
    0 0.8333 0
    ];
result_P * result_C
