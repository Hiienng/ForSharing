# Financial Model by Machine Learning
#BankDomain, #FinancialModel, #LoanAnalysis

SAP là một mô hình dự đoán tương lai của một sản phẩm cụ thể (UPL/Card/TD...). 
Mục đích: Phục vụ cho việc tính pricing và rộng hơn là thiết kế một nhánh sản phẩm mới dựa trên các dữ liệu product owner đang kế hoạch.

Để biết được liệu sản phẩm mới được xây dựng có lợi nhuận không. Mô hình vẽ ra viễn cảnh tương lại dựa trên những thông tin phi tài chính mà đội Product lập lên cho sản phẩm. Ở mỗi doanh nghiệp, mô hình được thay đổi dựa vào customer journey, năng lực hệ thống và nhu cầu của người xây dựng mô hình. Tại V chúng tôi xây dựng mô hình như sau:

- Dữ kiện input (giả định và thực tế):
   - REVENUE INPUT
   - COST INPUT
   - RISK INPUT
- Dữ kiện output (dự đoán):
   - BALANCE OUTPUT
   - PNL OUTPUT

## 1. Thuật ngữ

1. EQP - End Quantity Pricing: Lãi suất tính được từ đầu
2. EMI - EQUAL MONTHLY INSTALLMENT: Lãi suát phát sinh tùy kỳ
3. Loan amount: Số tiền cho vay
4. LTV - Loan to Value: 
    - LTV - Loan to Value= 100%
    - Tỷ lệ này cho biết số tiền vay bằng đúng với giá trị của tài sản thế chấp. Điều này có nghĩa là khách hàng vay 100% giá trị của tài sản thế chấp.
5. Nominal Tenor = 18: Thời gian cover COF
6. Maturity to Termination = 3: Thời gian tạo lãi
7. MOB: Thời gian 
8. Attrition: 

## 2. Dữ kiện input
### 2.1. REVENUE INPUT
- Nhóm phi tài chính - Sản phẩm
   - a. Loan amount - Giá trị khoản vay
   - b. LTV - Giá trị thế chấp
   - c. Payment method
   - d. Nominal Tenor
   - e. Maturity to Termination
   - f. MOB - Month on book
   
   Ví dụ:
   |A | B | C | D | E | F |
   |------|----|-------|-------|-------|-------|
   | 40 mil    | 100%  | EQP | 18 months | 3 months | 21 months |
   
   ##### *MOB ở đây = Nominal Tenor + Maturity to Termination

- Pricing - Lãi suất cho vay (kế hoạch)

   Ví dụ:
   | From | To | IR    | COF   | NIM   |
   |------|----|-------|-------|-------|
   | 1    | 3  | 12.0% | 8.00% | 4.00% |
   | 4    | 21 | 12.0% | 8.00% | 4.00% |
   
- Early prepayment
   
   Ví dụ:
   | From | To | Fully | Partially | Penalty fee | Attrition |
   |------|----|----|-------|-------|-------|
   | 1 | 12 | 0.96% | 4.87% | 4.73% | 1.95% |
   | 13 | 24 | 0.91% | 3.17% | 4.23% | 2.22% |

- 3Ps commision
- Credit insurance
- ...

### 1.2. COST INPUT
- **Inflation rate:** 4%

-  Acquisition

   Ví dụ:
   | Description             | Cost (mil)|    Unit |
   |-------------------------|-------|-------------|
   | Sales incentive         | 0.06  | per account |
   | Sale team               | 0.20  | per account |
   | Sale regional mgt       | 0.02  | per account |
   | Marketing               | -     | per account |
   | OS Service Fee          | -     | per account |
   | Lead Acquisition        | -     | per account |

- UW & Disbursal

   Ví dụ:
   | Description             | Cost                   |
   |-------------------------|------------------------|
   | Underwriting cost       | 0.01 of application    |
   | Score                   | - of account           |
   | Disbursement cost       | 0.01 of disbursed application |
   | Approval rate           | 90.0% of submitted application |
   | Disbursement rate       | 98% of approved application |

- Collection & AMC

   Ví dụ:
   | Description             | Cost                   |
   |-------------------------|------------------------|
   | Collections_B0          | 0.01 per account/month |
   | Collections_B1+         | - per account/month    |
   | AMC_principal collected | - per 1m VND collected amount |
   | AMC_interest collected  | - per 1m VND collected amount |
   | AMC_penalty collected   | - per 1m VND collected amount |

- Supporting units

   Ví dụ:
   | Description               | Cost    |        Unit    |
   |---------------------------|---------|----------------|
   | IT                        | 0.01 | per account/month | 
   | Operation mgt             | 0.00 | per account/month |
   | After-sales mgt           | -    | per account/month |
   | Risk & Compliance         | 0.01 | per account/month |
   | Retail sale support & Mgt | 0.02 | per account/month |
   | Other support units       | 0.01 | per account/month |

### 1.3. RISK INPUT


## 3. Dữ kiện output
- **GENERAL OUTPUT**
   |                    | Life time | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 o/w | % Loan amt | % ANR |
   |--------------------|-----------|--------|--------|--------|--------|------------|------------|-------|
   | ANR                | 15.43     | 24.84  | 2.88   | -      | -      | -          | 38.6%      |       |
   | TOI                | 1.16      | 1.06   | 0.10   | -      | -      | -          | 1.7%       | 4.30% |
   | PROVISION          | -         | -      | -      | -      | -      | -          | 0.0%       | 0.00% |
   | OPEX               | (1.07)    | (0.84) | (0.23) | -      | -      | -          | -1.5%      | -3.94% |
   | Acquisition cost   | (0.28)    | (0.28) | -      | -      | -      | -          | -0.4%      | -1.02% |
   | UW & Disbursal     | (0.02)    | (0.02) | -      | -      | -      | -          | 0.0%       | -0.08% |
   | Maintenance cost   | (0.77)    | (0.54) | (0.23) | -      | -      | -          | -1.1%      | -2.84% |
   | PBT                | 0.09      | 0.22   | (0.13) | -      | -      | -          | 0.1%       | 0.35% |
   | PROFIT BEFORE SU   | 0.76      | 0.69   | 0.08   | -      | -      | -          | 1.1%       | 2.83% |

- **PERFORMANCE OUTPUT**
   |                          | Life time | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 o/w | % Loan amt | % ANR |
   |--------------------------|-----------|--------|--------|--------|--------|------------|------------|-------|
   | ANR                      | 15.43     | 24.84  | 2.88   | -      | -      | -          | 38.57%     |       |
   | TOI                      | 1.16      | 1.06   | 0.10   | -      | -      | -          | 1.66%      | 4.30% |
   | Net interest income      | 1.16      | 1.06   | 0.10   | -      | -      | -          | 1.66%      | 4.30% |
   | Interest accrual         | 3.24      | 2.98   | 0.26   | -      | -      | -          | 4.63%      | 12.00% |
   | Interest suspended       | (0.29)    | (0.24) | (0.05) | -      | -      | -          | -0.41%     | -1.07% |
   | Interest reinstated      | 0.18      | 0.14   | 0.04   | -      | -      | -          | 0.25%      | 0.66% |
   | Cost of fund             | (2.16)    | (1.99) | (0.17) | -      | -      | -          | -3.09%     | -8.00% |
   | Early termination income | 0.19      | 0.17   | 0.02   | -      | -      | -          | 0.27%      | 0.71% |
   | OPEX                     | (1.07)    | (0.84) | (0.23) | -      | -      | -          | -1.52%     | -3.94% |
   | Acquisition cost         | (0.28)    | (0.28) | -      | -      | -      | -          | -0.39%     | -1.02% |
   | Sales incentive          | (0.06)    | (0.06) | -      | -      | -      | -          | -0.08%     | -0.21% |
   | Sale team                | (0.20)    | (0.20) | -      | -      | -      | -          | -0.29%     | -0.75% |
   | Sale regional management | (0.02)    | (0.02) | -      | -      | -      | -          | -0.02%     | -0.06% |
   | Supporting Units         | (0.67)    | (0.46) | (0.21) | -      | -      | -          | -0.96%     | -2.48% |
   | IT                       | (0.14)    | (0.10) | (0.04) | -      | -      | -          | -0.20%     | -0.52% |
   | Operation mgt            | (0.03)    | (0.02) | (0.01) | -      | -      | -          | -0.05%     | -0.12% |
   | Risk & Compliance        | (0.10)    | (0.07) | (0.03) | -      | -      | -          | -0.14%     | -0.36% |
   | Retail sale support & Mgt| (0.23)    | (0.16) | (0.07) | -      | -      | -          | -0.33%     | -0.86% |
   | Other support units      | (0.16)    | (0.11) | (0.05) | -      | -      | -          | -0.23%     | -0.60% |
   | PBT                      | 0.09      | 0.22   | (0.13) | -      | -      | -          | 0.14%      | 0.35% |

- **FIXED & VARIABLE COST CLASSIFICATION**
   |                      | Life time | Year 1 | Year 2 | Year 3 | Year 4 | Year 5 o/w | % Loan amt | % ANR |
   |----------------------|-----------|--------|--------|--------|--------|------------|------------|-------|
   | TOI                  | 1.16      | 1.06   | 0.10   | -      | -      | -          | 2%         | 4.30% |
   | Variable Cost        | (0.18)    | (0.16) | (0.02) | -      | -      | -          | 0%         | -0.66% |
   | Provision cost       | -         | -      | -      | -      | -      | -          | 0%         | 0.00% |
   | PROFIT BEFORE FIXED  | 0.98      | 0.90   | 0.08   | -      | -      | -          | 1%         | 3.64% |
   | Fixed cost           | (0.89)    | (0.68) | (0.21) | -      | -      | -          | -1%        | -3.29% |
   | PBT                  | 0.09      | 0.22   | (0.13) | -      | -      | -          | 0%         | 0.35% |

## 4. Cơ chế xây dựng
