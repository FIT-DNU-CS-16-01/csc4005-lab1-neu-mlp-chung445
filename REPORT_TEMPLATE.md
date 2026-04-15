# CSC4005 – Lab 1 Report Template

## 1. Mục tiêu
Mục tiêu của lab là xây dựng và so sánh các cấu hình MLP khác nhau cho bài toán phân loại lỗi bề mặt thép NEU-CLS. Bộ dữ liệu NEU-CLS gồm 6 lớp lỗi: Crazing, Inclusion, Patches, Pitted_Surface, Rolled-in_Scale, Scratches.

## 2. Cấu hình thí nghiệm
Đã chạy 5 cấu hình với các yếu tố khác nhau:

| Run Name | Optimizer | LR | Weight Decay | Dropout | Patience | Best Val Acc | Test Acc | Best Val Loss |
|----------|-----------|----|-------------|---------|----------|-------------|----------|---------------|
| **baseline_adamw** | adamw | 0.001 | 0.0001 | 0.3 | 5 | **41.85%** | **38.15%** | **1.4993** |
| baseline_sgd | sgd | 0.001 | 0.0001 | 0.3 | 5 | 24.81% | 25.93% | 1.7297 |
| baseline_adamw_higher_reg | adamw | 0.001 | 0.001 | 0.5 | 5 | 38.15% | 36.30% | 1.6226 |
| baseline_adamw_lr_01 | adamw | 0.01 | 0.0001 | 0.3 | 5 | 26.67% | 22.22% | 1.7575 |
| baseline_adamw_patience_10 | adamw | 0.001 | 0.0001 | 0.3 | 10 | 41.85% | 38.15% | 1.4993 |

## 3. Kết quả
**Learning Curves**: Xem chi tiết trên W&B dashboard tại https://wandb.ai/chungbestriven-dainam-vietnam/csc4005-lab1-neu-mlp

**Quan sát chính:**
- AdamW vượt trội hơn SGD (41.85% vs 24.81% val acc)
- LR cao (0.01) khiến model không hội tụ tốt
- Regularization cao giảm overfitting nhưng hiệu suất thấp hơn
- Patience cao cho phép train lâu hơn, đạt kết quả tương đương

## 4. Phân tích
- **Cấu hình nào tốt nhất?** `baseline_adamw` với val acc 41.85%, val loss 1.4993
- **Dấu hiệu overfitting/underfitting:**
  - Overfitting: train loss giảm nhưng val loss tăng, train acc cao hơn val acc nhiều
  - Underfitting: cả train và val loss đều cao, accuracy thấp
  - Trong thí nghiệm: baseline_adamw có learning curve ổn định, không overfitting rõ rệt
- **AdamW vs SGD:** AdamW hội tụ nhanh hơn và đạt accuracy cao hơn đáng kể (41.85% vs 24.81%)

## 5. Kết luận
**Best config**: `baseline_adamw` (AdamW, LR=0.001, weight_decay=0.0001, dropout=0.3, patience=5)

**Lý do chọn:**
- Val accuracy cao nhất (41.85%)
- Val loss thấp nhất (1.4993)
- Test accuracy ổn định (38.15%)
- Learning curve ổn định, không overfitting
- So với các config khác: SGD kém hơn, LR cao không hội tụ, regularization cao quá mạnh
