# Costs 

## Itemized monthly cost breakdown

| Service                        | Quantity | Calculation                       | Monthly Cost (USD) |
|--------------------------------|----------|-----------------------------------|--------------------|
| VPC	                         | 1        | 1 × $0/month                      | $0                 |
| Internet Gateway	             | 1        | 1 × $0/month	                    | $0                 |
| Subnets					     | 6 		| 6 × $0/month	                    | $0                 |
| Route Tables	                 | 4        | 4 × $0/month	                    | $0                 |
| EC2 t3.micro instances	     | 4        | 4 × $0.0104/hr × 730 hrs	        | $30.37             |
| EC2 EBS gp3 storage	         | 4        | 4 × 8 GB × $0.08/GB-month         | $2.56              |
| Application Load Balancer	     | 1        | 1 × $0.0225/hr × 730 hrs	        | $16.43             |
| NAT Gateway hourly cost	     | 1        | 1 × $0.045/hr × 730 hrs	        | $32.85             |
| NAT Gateway data processing    | 100 GB   | 100 GB × $0.045/GB	            | $4.50              |     
| RDS db.t4g.micro compute	     | 2 ins.   | 2 × $0.017/hr × 730 hrs	        | $24.82             |
| RDS gp3 storage                | 100 GB   | 100 GB × $0.115/GB-month          | $11.50             |
| RDS backups	                 | NA       | Included within allocated storage	| $0                 |
| CloudWatch basic metrics	     | NA       | Included	                        | $0                 |
| VPC Flow Logs                  | NA       | NA                                | ~$5                |

**Total monthly cost :** ~$128 USD      

## Cost optimization strategies

- Enable auto-scaling to launch only sufficient instances to keep the system working. 
- Add VPC Endpoints for services such as RDS, Systems Manager, CloudWatch to reduce NAT Gateway usage. 
- Apply Saving Plans for stable EC2 workloads to reduce On-demand costs. 
- If acceptable for non-critical workloads, run a Single-AZ RDS instance instead of Multi-AZ (roughly halves the RDS compute cost)
- Optimize logging retention
- Revisit instance sizes after collecting 30–60 days of CloudWatch metrics

## ROI analysis for optimizations

| Optimization	                     | Monthly Saving   | Annual Saving |Implementation Effort	| Risk   |
|------------------------------------|------------------|---------------|-----------------------| -------|
| Add VPC Endpoints	                 | $5–20	        | $60–240       | Low	                | Low    |
| Use Savings Plans	                 | $8–15	        | $96–180       | Low	                | Low    |
| Switch t3.micro → t4g.micro	     | $5–8	            | $60–96	    | Medium	            | Medium |
| Right-size RDS	                 | $10–20	        | $120–240	    | Medium	            | Medium |
| Reserved DB Instance	             | $8–15	        | $96–180	    | Low	                | Low    |
| Remove idle EC2 instances	         | Variable	        | Variable	    | Medium	            | Medium |
| Use private service endpoints	     | $5–15	        | $60–180	    | Medium	            | Low    |
| Reduce logging noise	             | $5–10	        | $60–120	    | Medium	            | Low    |

