
<img width="1536" height="1024" alt="0f59b8fd-9bce-415e-b567-a9ffd133bc2b" src="https://github.com/user-attachments/assets/b30d758d-3cf5-41e6-bdec-1919b7993daa" />


**1.ARCHITECTURE SECTION**

**## 🏗️ System Architecture<img width="741" height="352" alt="MSEDg42xjyydWTbGF2dRcty2IiZdrWS4MdPmibUSHxiR9gnyctubapQdvOXepX57YpNfTiTnSEdId-QWYDa1HTOgAonlwGT5QH0Dmn7rFj_0FL6Cx9BmQy70jS7Fe6a_fwlktuwt4kUJi5SwyWaKdOspi0B6OMOy2iNInxOlRF9-s3fYmP3rvm72iEKLoh3c" src="https://github.com/user-attachments/assets/21faa1eb-8ba7-40b1-b4cf-0edab79f9ebf" />**
<img width="552" height="341" alt="IQZSr0QfDY2onJ9Eow3yoka-jVRazZRCHoBLFZn_pdZNGF5XHIopFTLHGqKZp8kKEJBopB0Vzx_xsy9qboKpEN_iz2aHcLk6OUrMiYAPTmzHT336bZ03GIBdjFbD8rg8E40jKWqurb0bMG_PuxnOMf--OfPgK-WvEXrKPsRz82FzUiB9iZbYilIhc_NCcLCc" src="https://github.com/user-attachments/assets/6fc9258e-a9ed-43ea-b70a-69454e47eddb" />
<img width="1000" height="420" alt="w5wqt3VGQalMQx3yeO2CrGqxgIFzOR2gAcuNBtuFV9oO_ZMRP09IFAHvf-RcVVt6A_7znSPzagYTMb2KTM6IooDpX6jIjHHxvwoTZJa-v_atzGeYN1wjfcVUUgPoW2tAVuiHGvvMXinhumU3j4vtLTzfwMH_oN3NBkwrrVD2UeSZ76Ei_MmQ24asOtjDoGjd" src="https://github.com/user-attachments/assets/6d009862-fda3-4795-9a60-c76ee9180f3f" />


This system is designed as a scalable backend rate limiter using Spring Boot.

### Flow:
1. Client sends API request
2. Controller receives request
3. RateLimiterService validates request
4. Algorithm (Token Bucket / Sliding Window) decides allow/block
5. Allowed requests are stored in database
6. Response returned to client

### Components:
- Controller → Handles API requests
- Service Layer → Core business logic
- Rate Limiting Algorithms → Token Bucket & Sliding Window
- Database → Stores request logs
- Redis (optional) → Enables distributed rate limiting

### Key Design Decisions:
- Used in-memory + Redis for scalability
- Separated algorithm logic from service layer
- Designed for high concurrency using thread-safe structures


**2. TOKEN BUCKET EXPLANATION**

<img width="1024" height="732" alt="0dhQ0aXg601CWl4-JCOCK7zDguQurQwYnRXieP0la8IOXkSkbrgu7P7xfy6acpWAv3OZUlMwTtjSKOE7QghXZcpV69XxYG8MPFiFGwa1aYvUBMl3NiX8iRMI9cLR0t1DjEXJhgmDAIUrHpFq2TWY8lOOOaG6ri_BzbN_J15BZ8ze63jueK9t4KyPUL7dOh0H" src="https://github.com/user-attachments/assets/e445ea57-500b-48c4-882c-cc3b35c09d73" />
<img width="1559" height="748" alt="VELkmvurMqsYZCI1EuRBaYXsodw3m-J3xsBICCKk2GtfcJGluLApYci8oLov2kT_QHF05XwGv9hSehitcG5YjECUGv4DFIntImIRCXnATrF7CAbzyNR2xMIujK43X1yfO69or6csTrbXLqnLIypj2No58EQq4dYs8ep7rgWMYAY2x014Vfej30zC8E4vCHaA" src="https://github.com/user-attachments/assets/1fb09f2c-8ce3-47cb-b07b-bba0a5dafa06" />
<img width="1400" height="788" alt="6Nlvv1IpDQVq0V8TnkkKW11jwxbtpBpPomURug6Y_iJFwcpDQZ8Uge2ZV8EgMf85iOOxgajKCCYgjI5j_0OGvVSY7IQHUtHPXnkxQi-24IZigZfSmD2N45CPDaTr1sAXBk0MjSBV-iCZmIfRWKJ6tDG1pvOpAKRKGSsraPlxc5L8Ym-wyWyomXYztwrdp9eM" src="https://github.com/user-attachments/assets/0b5b9d30-896d-4a97-a84c-24a7a1065f8c" />

## ⚙️ Token Bucket Algorithm

The Token Bucket algorithm allows burst traffic while maintaining a steady request rate.

### How it works:
- A bucket holds tokens (capacity = max requests)
- Each request consumes 1 token
- Tokens are refilled at a fixed rate
- If no tokens → request is blocked

### Why used:
- Handles burst traffic efficiently
- Simple and performant
- Suitable for real-time APIs

### Example:
- Capacity = 10 tokens
- Refill rate = 2 tokens/sec
- User can send bursts of 10 requests instantly


**3. SLIDING WINDOW EXPLANATION**

<img width="1042" height="745" alt="23JtWk_F0MJfSdBszEFWLyUOY0yDu3H418AP0Ne817UqWBtGO8hH0qMjlsoAgV7-cJd8Kp--EXE9jlypN0iD9Fw135JRZG2OQuyDa5dEfCR4FyqN5IQ0GhdbdMujxdiLMNbrLqrW9y_AXgfpWpwvRP5H2SQoPdzp-083_43McstnjZX4WfxaIDqgJZj4o3LG" src="https://github.com/user-attachments/assets/db802dee-ff8f-44b1-8448-d068a8bc0361" />
<img width="1200" height="773" alt="nlOmzoSpl8zMJjGJEXbopGcam82p51GcSDxtIAr29kKkNpuaycV6CrtCFrd29HCr6tq5vf0nrFPIBIHhvWK1xM-A4H7QorTd3UtM7obbbF5tC0BOflCoXw0e-y_mmnhGOHfncAsOZQw6_lc6WVeMqy8S6qTkU2icwNnEzjuG-CCE1T4DVZThPWeCAG3R5iFc" src="https://github.com/user-attachments/assets/e5374f8b-8c8d-45e8-a3e4-9ec9154f0ec1" />
<img width="1226" height="616" alt="fczp4hUuTA4Tl80b2-9HFaWjyB_2uiCDqUr5NjwNQB3kIp7NChuMclKYeOg2v2FFYKoe5vdAPcetgvJEpMsTaau90GvYMM3AbL5WGgL-kHYREdD9Jcg4Wu7jxT_q-CQbzMWIKK3fsCpOK506l97UV6rynjdZaKukzkUVi1pUm2Z40RxGrtqpnVNrAbIeYX6I" src="https://github.com/user-attachments/assets/4dd912d4-8273-4e54-a674-d52bd219bbcb" />


## ⏳ Sliding Window Algorithm

The Sliding Window algorithm enforces strict request limits over time.

### How it works:
- Maintains timestamps of recent requests
- Removes requests outside the time window
- Allows request only if within limit

### Why used:
- Provides accurate rate limiting
- Prevents burst abuse
- Better control than fixed window

### Example:
- Limit: 5 requests per 10 seconds
- 6th request within window → blocked


**4. REQUEST FLOW**
<img width="1200" height="630" alt="SpUmG4PL4fqX8siaqdkb9SvRbU8EfOS9-CpmMi24zObhsMFLKt2I-MLJsrK4NXAaDDufK4t_HPp5pykvM5xD_4vxbB4PqcH6Ezg_GDzIaMu5suB4pzF4qYzNkjia1UvoIZJCmHsoFgxq_gawtaSCokuenjUrulsQa2sKRa73V7NnfPLPR_HJpB90LwVgePbA" src="https://github.com/user-attachments/assets/1c2b6d03-7907-4741-82b9-67cadc827246" />
<img width="1024" height="972" alt="muwugLLV6ZBY5W1TfBPJ9xooxFr4UK-Dv_-iQJ5hBgZVy5F6Feyo6bj9JlbC1_0bPputt1w4mZoms01YEf6CxN0kqhTYwBHKEgThtWsiee61YoYDI2u1YBOKB8qJMK1Bl9V4Ix5PNwKr8QiQXk_XvMgVTTq5Sml34b1mJ_oN-3MRNz2C_9-TPcQDfB6wtKqC" src="https://github.com/user-attachments/assets/ddbfbc25-2ff6-4acf-a82d-85c5d6a4da3e" />
<img width="732" height="648" alt="EauZgk-7RgkShgproc6B494426dVG8dRN2hiqnJf-ALVckFOq8CIHojtgv90WnQH8IdfWV1a5xf99Fz4CLaTqA3tc9o8H6idmK_uFsqkjd9b1PWsgqLjZfu5C_fnp202slYZMsJo_p6NZBA3ZjKhov5T9fGL2C1AjPXVTf91T5UbKF7gyJE5plmWJ_p5ST3U" src="https://github.com/user-attachments/assets/4055f64b-0a41-4127-ba64-09f011912708" />
<img width="570" height="331" alt="Uzu7Zj8sik6A9mYUVT9N0VOeo3u0OM-Dqcyyc2sxwYp0FJampNbjbK5UeEKTUsegaLoZKq_MdNT3SGGkzqyHL3wS7569nb8tLOlmRdDbogKqlOTnVqIR520eRPwAj7S3bbNY6GRO4YgwHCFclDdb41gcwz5v-JK12qgj5IASCMs_Lwsxdm0ATtujsuGRKRdp" src="https://github.com/user-attachments/assets/934fa69e-5ae4-4596-96cf-f6505e7bf007" />


## 🔁 Request Flow

1. User sends request with userId
2. Controller receives request
3. Service checks global rate limit
4. Service checks per-user rate limit
5. If allowed:
   - Request stored in database
   - Response = Allowed
6. If blocked:
   - Response = Rate Limit Exceeded

### Key Highlights:
- Thread-safe request handling
- Supports concurrent users
- Ensures fairness across users

5. CONCURRENCY DESIGN
<img width="637" height="325" alt="4wDTig6GMcdi0GcCAMMYo0wjg-QgNvTqdgLHnCC_AwmhkUAWkZoe5K5Z0iBuFMqz30QLT9ZMOrKcDsVdwCVHeNYGU0Xo2Jd3IWC_sb6DhoG1Tpx-d7Av9pKqeVdiwnuAMxlq6qHxcqDT_m-yAxALx5T91CmvEKc3XEXaUOPygMaYmERqTKPPjayLgo716pTT" src="https://github.com/user-attachments/assets/5223a4bf-c831-46ff-8252-50b1883ced9e" />
<img width="1308" height="730" alt="QCy5-NLzfNu2SIHh1VhnKYHnH8JyNxSFSWd65WZfNV4NByTTNPkD4mW4OPMbuC5Q0OSzr2KDkmiXhWPC8ezDWDZlND5bqiMuUfQQyEbp9DFrLISnbzIjlmjXsEYZSrekmboeKUSWEPCmwrblDQRf5BD4iVDVDD30rZJLm2GnBiNSH3jwDq5-tekLsO1E8Jdw" src="https://github.com/user-attachments/assets/168eb718-2801-48e9-8b23-72fa732e4294" />


## 🧠 Concurrency Handling

The system is designed to handle multiple concurrent requests safely.

### Techniques used:
- ConcurrentHashMap → Thread-safe storage for user buckets
- synchronized methods → Prevent race conditions
- Atomic variables → Ensure accurate token updates

### Why important:
- Multiple users can hit API simultaneously
- Prevents inconsistent state
- Ensures reliable rate limiting

### Example:
- 100 users hitting API at same time
- Each user handled independently
- No data corruption occurs

**FINAL SECTION**
## Key Features

- Thread-safe rate limiting system
- Supports Token Bucket & Sliding Window algorithms
- Per-user and global rate limiting
- Scalable design (Redis ready)
- Real-time request tracking with database
- Built using Spring Boot and Core Java

## Future Enhancements

- Redis integration for distributed systems
- API Gateway integration
- Admin dashboard (React)
- Metrics monitoring (Prometheus/Grafana) 
