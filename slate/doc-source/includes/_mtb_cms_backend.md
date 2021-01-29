# Enigma Admin Backend API

> Artisan command:

```php
cd /path/to/enigma-admin_backend

php artisan
```
- [Product requirements](https://neo-universe.atlassian.net/wiki/spaces/ENIGMA/pages/85117753/Services)

- [Bitbucket](https://bitbucket.org/nldanang/enigma-admin_backend)

- Basic authentication: `enigma/cie1yiVa`


## Bank response
**1. URL**

- POST: {URL}/transaction/verify

**2. Description**

- Receive bank response data

**3. Last modify**

	// TO DO

**4. Parameter**

- receiptId:
    + required
    + alpha_num
    + min:15
    + max:15
- tranId:
    + required
    + is_halfwidth_number
    + digits:20
- transferDate:
    + nullable
    + date_format:Ymd
    + min:8
    + max:8
- cBankCode:
    + required
    + is_halfwidth_number
    + digits:4
- cBankName:
    + nullable
    + max:15
- cBranchCode:
    + required
    + is_halfwidth_number
    + digits:3
- cBranchName:
    + nullable
    + max:15
- cAccountType:
    + required
    + integer
    + digits:1
- cAccountNumber:
    + required
    + is_halfwidth_number
    + digits:7
- cAccountName:
    + required
    + max:30
- transferName:
    + required
    + max:48
- rBankCode:
    + required
    + is_halfwidth_number
    + digits:4
- rBankName:
    + nullable
    + max:15
- rBranchCode:
    + required
    + is_halfwidth_number
    + digits:3
- rBranchName:
    + nullable
    + max:15
- rAccountType:
    + required
    + integer
    + digits:1
- rAccountNumber:
    + required
    + is_halfwidth_number
    + digits:7
- rAccountName:
    + required
    + max:48
- amount:
    + required
    + integer
    + digits_between:1,10
- transferType:
    + required
    + integer
    + digits:2
- partnerCode:
    + required
    + is_halfwidth_number
    + digits:10
- userData1:
    + nullable
    + max:20
- userData2:
    + nullable
    + max:20
- hashCd:
    + required
    + max:64

> **5. Response**

```php
{
    status_code
    message
    messageId
}
```

**6. Code handle**

- Controller - function:
	+ BankResponseController@receiveBankResponse
- Service:
    + App\Services\BankApis\ResponseService
- Repository:
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Login
### Login
**1. URL**

- POST: {URL}/authenticate

**2. Description**

- Login enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

- email:
	+ required
	+ email
	+ max:255
- password:
    + require_trim
    + type_password
	+ length(6, 255)

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Login success",
        data: {
            token
            user_info: {
                id
                role
                email
                name
                name_kana
                stopped_at
                stop_reason
                last_login_at
                ip
                created_at
                role_name
            }
        }
    }

- 401:
    {
        status_code: 401,
        message: "Your login information was incorrect.",
        errors: []
    }

- 422:
    {
        status_code: 422    ,
        message: "Your login information was incorrect.",
        errors: []
    }

- 500:
    {
        status_code: 500,
        message: "Your login information was incorrect.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ AuthenticateController@login
- Service:
- Repository:
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Authenticate with 2 factor
**1. URL**

- POST: {URL}/authenticate/google/callback

**2. Description**

- Handle google callback

**3. Last modify**

	// TO DO

**4. Parameter**

- redirectUri: nullable
- user_id:
- secret_token:

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Login success",
        data: {
            token,
            user_info: {
                id
                role
                email
                name
                name_kana
                stopped_at
                stop_reason
                last_login_at
                ip
                created_at
                role_name
        }
    }

- 401:
    {
        status_code: 401,
        message: "Your login information was incorrect.|The login time has expired, please login again.",
        errors: []
    }

- 422:
    {
        status_code: 422,
        message: "Invalid input data.",
        errors: []
    }
    
- 403:
    {
        status_code: 403,
        message: "You do not have access to this page.",
        errors: []
    }

- 500:
    {
        
        status_code: 500,
        message: "Your login information was incorrect.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ AuthenticateController@handleGoogleCallback
- Service:
    + App\Services\GoogleOauthService
- Repository:

**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Password
### Email reset password
**1. URL**

- POST: {URL}/password/email

**2. Description**

- Get link reset password email.
- Notice: This function will send link reset password email for manager who reset password (token, url, name).

**3. Last modify**

	// TO DO

**4. Parameter**

- email:
	+ required
	+ email
	+ exists:enigma_users,email,deleted_at,NULL

> **5. Response**

```php
- 200:
    {
        status: 200,
        message: "Send password reset success"
        data: []
    }


- 422:
    {
        status: 422,
        message: "Send password reset failed"
        errors: []
    }

- 500:
    {
        status: 500,
        message: "Send password reset failed"
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PasswordController@sendResetLinkEmail
- Repositories:
- Service:

**7. Table get data**

- enigma_user_password_resets

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**

	// TO DO

### Check url reset password
**1. URL**

- GET: {URL}/password/check-url

**2. Description**

- Check url reset password.

**3. Last modify**

	// TO DO

**4. Parameter**

- token:

> **5. Response**

```php
- 200:
	{
		status: 200,
		message: ""
		data: {
			is_valid: true 
		}
	}
	
- 200:
    {
        status: 200,
        message: ""
        data: {
            is_valid: false,
            message: "This URL is invalid."   
        }
    }

- 500:
	{
		status: 500,
		message: "Occur an error while checking password reset url."
		errors: []
	}
```

**6. Code handle**

- Controller - function:
	+ PasswordController@checkResetUrl
- Repositories:
    + App\Repositories\Eloquent\EnigmaUserPasswordResetRepository
- Service:
	+ App\Services\Password\CheckResetPasswordUrlService

**7. Table get data**

- enigma_user_password_resets

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**

	// TO DO

### Reset password
**1. URL**

- POST: {URL}/password/reset

**2. Description**

- Reset manager password by email.
- Notice: This function will send changed password mail (manager name and url login).

**3. Last modify**

	// TO DO

**4. Parameter**

- token:
	+ required
- email:
	+ required
	+ email
	+ exists:enigma_users,email,deleted_at,NULL
	+ exists:enigma_user_password_resets,email,token
- password:
	+ required
	+ require_trim
	+ type_password
	+ length(6, 255)
- password_confirmation:
	+ require_same:password

> **5. Response**

```php
- 200:
	{
		status: 200,
		message: "Reset password success"
		data: []
	}

- 422:
	{
		status: 422,
		message: "Reset password failed"
		errors: []
    }

- 500:
	{
		status: 500,
		message: "Reset password failed"
		errors: []
	}
```

**6. Code handle**

- Controller - function:
	+ PasswordController@reset
- Service:

**7. Table get data**

- enigma_user_password_resets

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**

	// TO DO

## Version api
**1. URL**

- GET: {URL}/version

**2. Description**

- API get version.

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php	
{
    message: "Enigma version release"
    data: {
        version: "v2.2.1"
    }
}
```

**6. Code handle**

	// TO DO

**7. Table get data**

	// TO DO

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Logout
**1. URL**

- GET: {URL}/logout

**2. Description**

- Logout enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php	
- 200:
    {
        status: 200,
        message: "logout success"
        data: []
    }
```    

**6. Code handle**

- Controller - function:
    + AuthenticateController@logout

**7. Table get data**

	// TO DO

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**

	// TO DO

## Common
### List period of employee payment request by company id
**1. URL**

- GET: {URL}/companies/{companyId}/schedule-selection

**2. Description**

- Get schedule selection by company id

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getScheduleSelectionByCompany
- Service:
    +  App\Services\CommonApis\ScheduleSelectionService
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    
**7. Table get data**

- company_payment_cycle

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List period of employee payment request by employee id
**1. URL**

- GET: {URL}/employees/{employeeId}/schedule-selection

**2. Description**

- Get schedule selection by employee id

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getScheduleSelectionByEmployee
- Service:
    + App\Services\CommonApis\ScheduleSelectionService
- Repository:
    + App\Repositories\Eloquent\EmployeeRepository
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    
**7. Table get data**

- company_payment_cycle

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List employee payment request types
**1. URL**

- GET: {URL}/employee-payment-requests/types

**2. Description**

- Get payment request type

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getPaymentRequestType
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List status of employee
**1. URL**

- GET: {URL}/employees/status

**2. Description**

- Get list status of employee

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message "",
        data: [
            {
                id
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getStatusEmployee
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List status of branch payment schedule
**1. URL**

- GET: {URL}/employee-payment-requests/status

**2. Description**

- List status of payment request

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getStatusPaymentRequest
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get csrf token
**1. URL**

- GET: {URL}/csrf-token

**2. Description**

- Get csrf token

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
{
    status_code
    message
    data
}
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getCsrfToken
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List type of company kintai
**1. URL**

- GET: {URL}/company-kintais/types

**2. Description**

- List type of company kintai

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@listCompanyKintaiTypes
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get role enigma user
**1. URL**

- GET: {URL}/enigma-users/roles

**2. Description**

- Get enigma user role

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id,
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getEnigmaUserRole
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get all company
**1. URL**

- GET: {URL}/companies/all

**2. Description**

- Get all company

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id,
                name,
                code
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getAllCompany
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List time periods for report
**1. URL**

- GET: {URL}/reports/list-time-periods

**2. Description**

- List time periods

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id,
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getTimePeriods
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get type of prepaid usage list type
**1. URL**

- GET: {URL}/prepaid-usage-types

**2. Description**

- Get type of prepaid usage list type

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id,
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getPrepaidUsagelistTypes
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get all company customer
**1. URL**

- GET: {URL}/real-companies/all

**2. Description**

- Get all real companies

**3. Last modify**

	// TO DO

**4. Parameter**

- company_name: nullable
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    name
                    code
                },
            ]
        }
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getAllRealCompanies
- Service:
    + App\Services\CommonApis\RealCompanyService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List deposit types
**1. URL**

- GET: {URL}/bank-deposit-types

**2. Description**

- Get deposit type

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id,
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getDepositType
- Service:
- Repository:
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    
**7. Table get data**

- enigma_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List ftp type
**1. URL**

- GET: {URL}/ftp-types

**2. Description**

- Get type of Ftp

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            id,
            name
        }
    }
```

**6. Code handle**

- Controller - function:
	+ CommonApiController@getFtpTypes
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Download- Export deductible employee csv
**1. URL**

- GET: {URL}/companies/{companyId}/branch-payment-requests/export-csv

**2. Description**

- Export deductible employee CSV

**3. Last modify**

	// TO DO

**4. Parameter**

- schedule_id
- branch_id
	
> **5. Response**

```php
- 200
 Returns the number of characters read from the handle and passed through to the output (file csv with name have format EmployeeDeductionsCSV_%s.csv)

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred while downloading CSV!",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PaymentRequestController@exportDeductibleCsv
- Service:
    + App\Services\PaymentRequestService
- Repository:
    + App\Repositories\Eloquent\EmployeeDeductionRepository
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    + App\Repositories\Eloquent\BranchRepository
    
**7. Table get data**

- employee_deductions
- employee_payment_requests
- branches

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Company cycle
### Get company cycle type
**1. URL**

- GET: {URL}/companies/cycle-type

**2. Description**

- Get types of company cycle

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            [
                id,
                name
            ]
        }
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@getCompanyCycleTypes
- Service:
- Repository:
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get timing by cycle type
**1. URL**

- GET: {URL}/companies/timing/{cycleType}

**2. Description**

- Get timing by cycle type

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            start_applying_date: [
                {
                    id
                    name
                }
            ],
            end_applying_date: [
                {
                    id
                    name
                }
            ],
            paying_salary_day: [
                {
                    id
                    name
                }
            ],
        }
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@getTiming
- Service:
    + App\Services\CompanyCycleService
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentCycleRepository
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Validate create company
**1. URL**

- POST: {URL}/companies/validate

**2. Description**

- Validate input data

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Validate data successfully",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Data validation failed",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@validateInputData
- Service:
    + App\Services\Companies\ValidateCreateService
    + App\Services\Companies\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\ManagerRepository
    + App\Repositories\Eloquent\CompanyPrepaidSettingRepository
    
**7. Table get data**

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update company prepaid setting
**1. URL**

- PUT: {URL}/companies/{companyId}/prepaid-setting

**2. Description**

- Update prepaid setting

**3. Last modify**

	// TO DO

**4. Parameter**

- upper_limit_rate:
    + required
    + integer
    + max:100
    + min:max_branch_upper_limit_rate
- upper_limit_total_per_day:
    + nullable
    + integer
    + min:max(max_branch_upper_limit_total_per_day, company_upper_limit_per_once)
- prepayment_fee:
    + required
    + numeric
    + min:0
    + max:9999
- insurance_fee:
    + required
    + integer
    + min:0
- upper_limit_prepayment_fee:
    + integer
    + min:0
    + nullable
- consumption_tax_flag:
    + required
    + boolean
- is_missing_amount:
    + required
    + boolean
- upper_limit_per_once:
    + integer
    + less_than_or_equal:upper_limit_total_per_day,true
    + required_with:lower_limit_per_once
- lower_limit_per_once:
    + integer
    + min:1000
    + less_than:upper_limit_per_once,true
    + required_with:upper_limit_per_once

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update prepaid setting.",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating the prepaid setting.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@updatePrepaidSetting
- Service:
    + App\Services\Companies\UpdatePrepaidSettingService
    + App\Services\Companies\CommonService
- Repository:
    + App\Repositories\Eloquent\BranchPrepaidSetting
    + App\Repositories\Eloquent\CompanyPrepaidSettingRepository
    
**7. Table get data**

- company_prepaid_setting

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update payment cycle setting
**1. URL**

- PUT: {URL}/companies/{companyId}/payment-cycle

**2. Description**

- Update company payment cycle setting

**3. Last modify**

	// TO DO

**4. Parameter**

- cycle_type:
    + required
    + in:CompanyPaymentCycle::CYCLE_MONTH, CompanyPaymentCycle::CYCLE_WEEK
- cycle_closing_kintai_day:
    + required
    + integer
    + in:5,10,15,20,25,30,31
- cycle_start_applying_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_end_applying_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_start_applying_month:
    + required
    + integer
    + in:CompanyPaymentCycle::VALIDATE_LAST_CURRENT
- cycle_end_applying_month:
    + required
    + integer
    + in:CompanyPaymentCycle::VALIDATE_CURRENT_NEXT
- cycle_closing_fee_day_after:
    + required
    + integer
    + between:1,10
- cycle_paying_salary_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_paying_salary_month:
    + required
    + integer
    + in:CompanyPaymentCycle::VALIDATE_CURRENT_NEXT
- cycle_end_applying_date_before:
    + required_if:cycle_end_applying_day,31

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update company cycle setting.",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating the company cycle setting.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@updatePaymentCycleSetting
- Service:
    + App\Services\CompanyService
    + App\Services\Companies\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\CompanyPaymentCycleRepository
    
**7. Table get data**

- company_payment_cycles

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Company
### Check company with exist information
**1. URL**

- POST: {URL}/check-bank-exists/{companiId?}

**2. Description**

- Check the bank information that already exists

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Validate data successfully",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Data validation failed",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@checkBankExist
- Service:
    + App\Services\Companies\CompanyBankService
    + App\Services\Companies\CommonService
    + App\Services\Companies\ValidateUniqueBankAccountNumberService
    + App\Services\Companies\ValidateUniqueBankUserIdService
- Repository:
    + App\Repositories\Eloquent\CompanyBankSettingRepository
    
**7. Table get data**

- company_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List companies
**1. URL**

- GET: {URL}/companies

**2. Description**

- List companies

**3. Last modify**

	// TO DO

**4. Parameter**

- company_code: nullable
- company_name: nullable
- is_with_trashed: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    name
                    name_kana
                    code
                    created_at
                    deleted_at
                    is_deleted
                    first_manager_admin: {
                        id
                        code
                        name
                        name_kana
                        email
                        company_id
                    }
                }
            ]
        }
    }
    
- 500:
    {
        
        status_code: 500,
        message: "Company cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create company
**1. URL**

- POST: {URL}/companies

**2. Description**

- Register a company

**3. Last modify**

	// TO DO

**4. Parameter**

- company_code:
    + required
    + type_code
    + max:20
    + unique_lower:companies,code,NULL,id,deleted_at,NULL
- company_name: 
    + required
    + max:40
- company_name_kana:
    + required
    + is_kana
    + max:40
- company_zip_code:
    + required
    + zip_code_number
    + fullsize
    + max:20
- company_address1:
    + required
    + max:255
- company_address2:
    + max:255
- company_department_name_first_part:
    + required
    + max:40
- company_department_name_middle_part:
    + nullable
    + max:40
- company_department_name_last_part:
    + nullable
    + max:40
- company_bank_user_id:
    + required
    + is_halfwidth_number
    + digits_between:10,10
- company_bank_name:
    + max:255
- company_bank_code:    
    + required
    + is_halfwidth_number
    + digits_between:4,4
    + exists:enigma_bank_settings,bank_code,deleted_at,NULL
- company_bank_branch_name:
    + max:255
- company_bank_branch_code:
    + required
    + is_halfwidth_number
    + digits_between:3,3
- company_deposit_type:
    + required
    + in:CompanyBankSetting::DEPOSIT_TYPE_NORMAL, CompanyBankSetting::DEPOSIT_TYPE_CURRENT, CompanyBankSetting::DEPOSIT_TYPE_SAVING
- company_bank_account_number:
    + required
    + is_halfwidth_number
    + digits_between:7,7
- company_bank_account_holder:
    + required
    + is_halfwidth_kana_extend
    + max:40
- company_transfer_name:
    + required
    + is_halfwidth_kana_extend
    + max:48
- company_bank_start_time:
    + required
    + date_format:Y-m-d,Y/m/d
    + after:today
- company_init_fee:
    + required
    + numeric
    + min:0
    + max:500000
- company_monthly_fee:
    + required
    + numeric
    + min:0
    + max:500000
- company_is_ftp_linked: 
    + required
    + boolean
- company_ftp_type:
    + required_if:company_is_ftp_linked,true,1
    + in:Company::FTP_KINTAI, Company::FTP_EMPLOYEE, Company::FTP_ALL
- company_sftp_directory:
    + required_if:company_is_ftp_linked,true,1
    + sftp_directory
    + max:255
- manager_code:
    + required
    + type_code
    + max:255
- manager_name:
    + required
    + max:40
- manager_name_kana:
    + required
    + is_kana
    + max:40
- manager_email:
    + required
    + email
    + max:255
- manager_phone_number:
    + nullable
    + phone_number
    + max:20
- manager_password:
    + require_trim
    + required
    + type_password
    + min:6
    + max:255
- manager_password_confirm:
    + require_same:manager_password
- setting_upper_limit_rate:
    + required
    + integer
    + max:100
    + min:1
- setting_upper_limit_total_per_day:
    + integer
    + min:1000
    + nullable
- setting_prepayment_fee:
    + required
    + numeric
    + min:0
    + max:9999
- setting_insurance_fee:
    + required
    + integer
    + min:0
- setting_upper_limit_prepayment_fee:
    + integer
    + min:0
    + nullable
- setting_consumption_tax_flag:
    + required
    + boolean
- setting_is_missing_amount:
    + required
    + boolean
- setting_upper_limit_per_once:
    + integer
    + less_than_or_equal:setting_upper_limit_total_per_day,true
    + required_with:setting_lower_limit_per_once
- setting_lower_limit_per_once:
    + integer
    + min:1000
    + less_than:setting_upper_limit_per_once,true
    + required_with:setting_upper_limit_per_once
- cycle_type:
    + required
    + in:CompanyPaymentCycle::CYCLE_MONTH, CompanyPaymentCycle::CYCLE_WEEK
- cycle_closing_kintai_day:
    + required
    + integer
    + in:5,10,15,20,25,30,31
- cycle_start_applying_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_end_applying_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_start_applying_month:
    + required
    + integer
    + in:1,3
- cycle_end_applying_month:
    + required
    + integer
    + in:1,2
- cycle_closing_fee_day_after:
    + required
    + integer
    + between:1,10
- cycle_paying_salary_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_paying_salary_month:
    + required
    + integer
    + in:1,2
- cycle_end_applying_date_before:
    + required_if:cycle_end_applying_day,31
    + in:0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Company has been added successfully.",
        data: {
            id
        }
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Create company error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@store
- Service:
    + App\Services\Companies\CreateService
    + App\Services\Companies\CommonService
    + App\Services\CompanyCycleService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\ManagerRepository
    + App\Repositories\Eloquent\CompanyPrepaidSettingRepository
    + App\Repositories\Eloquent\CompanyBankSettingRepository
    + App\Repositories\Eloquent\CompanyPaymentCycleRepository
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    
**7. Table get data**

- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update company
**1. URL**

- PUT: {URL}/companies/{companyId}

**2. Description**

- Update company info

**3. Last modify**

	// TO DO

**4. Parameter**

- code:
    + required
    + type_code
    + max:20
    + unique_lower:companies,code,$companyId,id,deleted_at,NULL
- name: 
    + required
    + max:40
- name_kana:
    + required
    + is_kana
    + max:40
- zip_code:
    + required
    + zip_code_number
    + fullsize
    + max:20
- address1:
    + required
    + max:255
- address2:
    + max:255
- department_name_first_part:
    + required
    + max:40
- department_name_middle_part:
    + nullable
    + max:40
- department_name_last_part:
    + nullable
    + max:40
- bank_user_id:
    + required
    + is_halfwidth_number
    + digits_between:10,10
- bank_name:
    + max:255
- bank_code:    
    + required
    + is_halfwidth_number
    + digits_between:4,4
    + exists:enigma_bank_settings,bank_code,deleted_at,NULL
- bank_branch_name:
    + max:255
- bank_branch_code:
    + required
    + is_halfwidth_number
    + digits_between:3,3
- deposit_type:
    + required
    + in:CompanyBankSetting::DEPOSIT_TYPE_NORMAL, CompanyBankSetting::DEPOSIT_TYPE_CURRENT, CompanyBankSetting::DEPOSIT_TYPE_SAVING
- bank_account_number:
    + required
    + is_halfwidth_number
    + digits_between:7,7
- bank_account_holder:
    + required
    + is_halfwidth_kana_extend
    + max:40
- transfer_name:
    + required
    + is_halfwidth_kana_extend
    + max:48
- bank_start_time:
    + required
    + date_format:Y-m-d,Y/m/d
    + after:today
- init_fee:
    + required
    + numeric
    + min:0
    + max:500000
- monthly_fee:
    + required
    + numeric
    + min:0
    + max:500000
- is_ftp_linked: 
    + required
    + boolean
- ftp_type:
    + required_if:company_is_ftp_linked,true,1
    + in:Company::FTP_KINTAI, Company::FTP_EMPLOYEE, Company::FTP_ALL
- sftp_directory:
    + required_if:company_is_ftp_linked,true,1
    + sftp_directory
    + max:255
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Company has been updated successfully.",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Update company error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@update
- Service:
    + App\Services\Companies\UpdateService
    + App\Services\Companies\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\CompanyBankSettingRepository
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    
**7. Table get data**

- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show company
**1. URL**

- GET: {URL}/companies/{id}

**2. Description**

- Show detail a company

**3. Last modify**

	// TO DO

**4. Parameter**
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            id
            name
            code
            name_kana
            zip_code
            address1
            address2
            department_name_first_part
            department_name_middle_part
            department_name_last_part
            is_ftp_linked
            ftp_type: {
                id
                name
            },
            sftp_directory
            prepaid_usage_list_type: {
                id
                name
            },
            manager_admins: [
                {
                    code
                    name
                    name_kana
                    email
                    company_id
                }
            ],
            prepaid_setting: {
                company_id
                upper_limit_rate
                upper_limit_total_per_day
                prepayment_fee
                insurance_fee
                upper_limit_prepayment_fee
                consumption_tax_flag
                is_missing_amount
                upper_limit_per_once
                lower_limit_per_once
            },
            company_payment_cycle: {
                company_id
                cycle_type
                cycle_closing_kintai_day
                cycle_start_applying_day
                cycle_end_applying_day
                cycle_start_applying_month
                cycle_end_applying_month
                cycle_closing_fee_day_after
                cycle_paying_salary_day
                cycle_paying_salary_month
                cycle_type_object: {
                    id
                    name
                },
                cycle_start_applying_month_object: {
                    id
                    name
                },
                cycle_end_applying_month_object: {
                    id
                    name
                },
                cycle_paying_salary_month_object: {
                    id
                    name
                },
                cycle_end_applying_date_before
            },
            company_bank_setting: {
                company_id
                bank_user_id
                bank_code
                bank_name
                bank_branch_code
                bank_branch_name
                deposit_type: {
                    id
                    name
                },
                bank_account_number
                bank_account_holder
                transfer_name
                bank_start_time
                init_fee
                monthly_fee
            }
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Company cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@show
- Service:
    + App\Services\Companies\ShowService
    + App\Services\Companies\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete company
**1. URL**

- DELETE: {URL}/companies/{id}

**2. Description**

- Delete a company

**3. Last modify**

	// TO DO

**4. Parameter**

- current_password:
    + required
    + current_password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Company has been deleted successfully.",
        data: []
    }
    
- 403:
    {
        status_code: 403,
        message: "The current password does not match.",
        errors: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Because of exist branches, you can not delete the company.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Company delete fail",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@destroy
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show current schedule of company
**1. URL**

- GET: {URL}/companies/{companyId}/schedules

**2. Description**

- Show detail of company payment schedule

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            closing_kintai_time
            start_applying_time
            end_applying_time
            closing_fee_time
            paying_salary_time
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Company payment schedule cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentScheduleController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- company_payment_cycles

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get list branch setting by company
**1. URL**

- GET: {URL}/companies/{companyId}/branches/upper-limit-rate-settings

**2. Description**

- Get list branch upper limit rate by company

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                branch_id
                branch_id
                name
                upper_limit_rate
            }
        ]
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@getBranchUpperLimitRate
- Service:
    + App\Services\BranchSettingService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- companies
- branches
- branch_prepaid_setting

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update upper limit rate for multiple branch setting
**1. URL**

- PUT: {URL}/companies/{companyId}/branches/upper-limit-rate-settings

**2. Description**

- Update upper limit rate for multiple branch

**3. Last modify**

	// TO DO

**4. Parameter**

- company_id:   
    + required
    + exists:companies,id,deleted_at,NULL
- data
    + data.*.branch_id: required|integer|exists:branches,id,company_id,$companyId
    + data.*.upper_limit_rate: required|integer|min:1|max:company_uopper_limit_rate
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update mutiple branch setting.",
        data: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating mutiple branch setting.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyController@updateBranchUpperLimitRate
- Service:
    + App\Services\BranchSettingService
- Repository:
    + App\Repositories\Eloquent\BranchPrepaidSettingRepository
    
**7. Table get data**

- branch_prepaid_setting

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO
    
### Preview company schedules
**1. URL**

- POST: {URL}/companies-schedules/previews/{companyId?}

**2. Description**

- Preview schedules of company

**3. Last modify**

    // TO DO

**4. Parameter**

- cycle_type:
    + required
    + in:CompanyPaymentCycle::CYCLE_MONTH,CompanyPaymentCycle::CYCLE_WEEK
- cycle_closing_kintai_day:
    + required
    + integer
    + in:5,10,15,20,25,30,31
- cycle_start_applying_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_end_applying_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_start_applying_month:
    + required
    + integer
    + in:CompanyPaymentCycle::VALIDATE_LAST_CURRENT
- cycle_end_applying_month:
    + required
    + integer
    + in:CompanyPaymentCycle::VALIDATE_CURRENT_NEXT
- cycle_closing_fee_day_after:
    + required
    + integer
    + between:1,10
- cycle_paying_salary_day:
    + required
    + integer
    + min:1
    + max:31
- cycle_paying_salary_month:
    + required
    + integer
    + in:CompanyPaymentCycle::VALIDATE_CURRENT_NEXT
- cycle_end_applying_date_before:
    + required_if:cycle_end_applying_day,31

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            date: {
                closing_kintai_time
                start_applying_time
                end_applying_time
                closing_fee_time
                paying_salary_time
            }
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Schedules cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
    + CompanyController@previewSchedules
- Service:
    + App\Services\Companies\PreviewScheduleService
    + App\Services\Companies\CommonService
    + App\Services\CompanyCycleService
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    
**7. Table get data**

- company_payment_schedules

**8. Table change data(insert, update, delete)**

    // TO DO

**9. SQL**
    
    // TO DO
    
## Branch
###  List branches of company
**1. URL**

- GET: {URL}/companies/{companyId}/branches

**2. Description**

-  List branches of company

**3. Last modify**

	// TO DO

**4. Parameter**

- sort_column: nullable
- sort_direction: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  [
            {
                id
                code
                name
                name_kana
                zip_code
                company_id
                address
                phone
                fax
                created_at
            }
        ]
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- companies
- branches

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

###  Create a branch of company
**1. URL**

- POST: {URL}/companies/{companyId}/branches

**2. Description**

-  Create a branch of company
-  Notice: Send mail when add branch admin success

**3. Last modify**

	// TO DO

**4. Parameter**

- code: 
    + required
    + type_code
    + unique:branches,code,NULL,id,company_id,{$companyId}
    + max:255
- name: 
    + required
    + max:40
    + unique:branches,name,NULL,id,company_id,{$companyId}
- name_kana:
    + is_kana
    + fullsize
    + max:40
- zip_code: 
    + zip_code_number
    + fullsize
    + max:20
- address: 
    + max:255
- phone:
    + phone_number
    + max:20
- fax: 
    + fax_number
    + max:20
- manager_code: 
    + max:255
    + type_code
    + unique:managers,code,NULL,id,company_id,{$companyId},deleted_at,NULL
    + required_with:manager_name,manager_name_kana,manager_email,manager_password,manager_password_confirmation
- manager_name: 
    + max:40
    + required_with:manager_code,manager_name_kana,manager_email,manager_password,manager_password_confirmation
- manager_name_kana:
    + max:40
    + required_with:manager_code,manager_name,manager_email,manager_password,manager_password_confirmation
- manager_email: 
    + email
    + max:255
    + required_with:manager_code,manager_name,manager_name_kana,manager_password,manager_password_confirmation
    + unique:managers,email,NULL,id,company_id,{$companyId},deleted_at,NULL
- manager_phone_number:
    + nullable
    + phone_number
    + max:20
- manager_password:
    + require_trim
    + min:6
    + max:255
    + required_with:manager_code,manager_name,manager_name_kana,manager_email
    + type_password
- manager_password_confirmation: 
    + require_same:manager_password
- manager_ids:
    + integer
    + exists:managers,id,company_id,{$companyId},role,Manager::IS_NOT_COMPANY_ADMIN
    + nullable
                              
> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Branch has been added successfully.",
        data:  []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Create branch error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchController@store
- Service:
    + App\Services\Branches\AddBranchService
- Repository:
    + App\Repositories\Eloquent\BranchRepository
    + App\Repositories\Eloquent\BranchAdminRepository
    + App\Repositories\Eloquent\BranchPrepaidSettingRepository
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    + App\Repositories\Eloquent\BranchPrepaidScheduleRepository
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branches

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

###  Update branch information
**1. URL**

- PUT: {URL}/companies/{companyId}/branches/{branchId}

**2. Description**

-  Update branch information

**3. Last modify**

	// TO DO

**4. Parameter**

- code: 
    + required
    + type_code
    + unique:branches,code,$branchId,id,company_id,{$companyId}
    + max:255
- name: 
    + required
    + max:40
    + unique:branches,name,$branchId,id,company_id,{$companyId}
- name_kana:
    + is_kana
    + fullsize
    + max:40
- zip_code: 
    + zip_code_number
    + fullsize
    + max:20
- address: 
    + max:255
- phone:
    + phone_number
    + max:20
- fax: 
    + fax_number
    + max:20
                              
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Branch has been updated successfully.",
        data:  []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Create branch error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchController@update
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchRepository
    
**7. Table get data**

- branches

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

###  Show detail of branch
**1. URL**

- GET: {URL}/companies/{companyId}/branches/{branchId}

**2. Description**

-  Show detail of branch

**3. Last modify**

	// TO DO

**4. Parameter**
                              
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            code
            name
            name_kana
            zip_code
            company_id
            address
            phone
            fax
            created_at
            branch_admins: [
                {
                    id
                    code
                    email
                    name
                    name_kana
                    phone_number
                }
            ],
            branch_managers: [
                {
                    id
                    code
                    email
                    name
                    name_kana
                    phone_number
                }
            ],
            company: {
                id
                code
                name
                name_kana
                zip_code
                address1
                address2
                department_name_first_part
                department_name_middle_part
                department_name_last_part
            }
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchRepository
    
**7. Table get data**

- branches

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

###  Delete a branch
**1. URL**

- DELETE: {URL}/companies/{companyId}/branches/{branchId}

**2. Description**

-  Delete a branch

**3. Last modify**

	// TO DO

**4. Parameter**
             
- current_password:
    + required
    + current_password
                 
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Branch has been deleted successfully.",
        data:  []
    }

- 403:
    {
        status_code: 403,
        message: "The current password does not match.",
        errors: []
    }
        
- 422:
    {
        status_code: 422,
        message: "Because of exist employees, you can not delete the branch.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch delete fail",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchController@destroy
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchRepository
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- branches

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

###  Show branch admin
**1. URL**

- GET: {URL}/companies/{companyId}/branches/{branchId}/branch-admin/{branchAdminId}

**2. Description**

-  Show branch admin

**3. Last modify**

	// TO DO

**4. Parameter**
                 
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            code
            email
            name
            name_kana
            role
            status
            phone_number
            created_at
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch admin cannot be listed",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchAdminController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchAdminRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create branch admin.
**1. URL**

- POST: {URL}/companies/{companyId}/branches/{branchId}/branch-admin

**2. Description**

-  Create branch admin.
-  Notice: Send email to manager that is created

**3. Last modify**

	// TO DO

**4. Parameter**
      
- code:
    + max:20
    + type_code
    + unique:managers,code,NULL,id,company_id,{$branch->company_id},deleted_at,NULL
    + required
- name:
    + max:40
    + required_with:code,name_kana,email,password,password_confirmation
- name_kana:
    + max:40
    + is_kana
    + fullsize
    + required_with:code,name,email,password,password_confirmation
- email:
    + email
    + max:255
    + required
    + unique:managers,email,NULL,id,company_id,{$branch->company_id},deleted_at,NULL
- phone_number:
    + nullable
    + phone_number
    + max:20
- password:
    + require_trim
    + min:6
    + max:255
    + required
    + type_password
- password_confirmation:
    + require_same:password
       
> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Successfully added administrator.",
        data:  {
            id
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "There was an error adding the administrator.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchAdminController@store
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchAdminRepository
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update branch admin.
**1. URL**

- PUT: {URL}/companies/{companyId}/branches/{branchId}/branch-admin/{branchAdminId}

**2. Description**

-  Update branch admin.

**3. Last modify**

	// TO DO

**4. Parameter**
      
- code:
    + max:20
    + type_code
    + unique:managers,code,$branchAdminId,id,company_id,{$branch->company_id},deleted_at,NULL
    + required
- name:
    + max:40
    + required_with:code,name_kana,email,password,password_confirmation
- name_kana:
    + max:40
    + is_kana
    + fullsize
    + required_with:code,name,email,password,password_confirmation
- email:
    + email
    + max:255
    + required
    + unique:managers,email,$branchAdminId,id,company_id,{$branch->company_id},deleted_at,NULL
- phone_number:
    + nullable
    + phone_number
    + max:20
- password:
    + require_trim
    + min:6
    + max:255
    + required
    + type_password
- password_confirmation:
    + require_same:password
       
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully updated administrator.",
        data:  []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "There was an error updating the administrator",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchAdminController@update
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchAdminRepository
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete branch admin.
**1. URL**

- DELETE: {URL}/companies/{companyId}/branches/{branchId}/branch-admin/{branchAdminId}

**2. Description**

- Delete branch admin.

**3. Last modify**

	// TO DO

**4. Parameter**

- current_password:
    + required
    + current_password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully deleted administrator. ",
        data:  []
    }

- 403:
    {
        status_code: 403,
        message: "The current password does not match.",
        errors: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred deleting the administrator.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchAdminController@destroy
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchAdminRepository
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Add branch admin by id
**1. URL**

- POST: {URL}/companies/{companyId}/branches/{branchId}/add-branch-admin/{managerId}

**2. Description**

- Add branch admin by id

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully added administrator. ",
        data:  []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch admin cannot be created.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchAdminController@addBranchAdminId
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchAdminRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Detach branch admin by id
**1. URL**

- POST: {URL}/companies/{companyId}/branches/{branchId}/detach-branch-admin/{managerId}

**2. Description**

- Detach manager out branch.

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully deleted the administrator.",
        data:  []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Failed to delete administrator.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchAdminController@detachBranchAdmin
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchAdminRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show branch manager
**1. URL**

- GET: {URL}/companies/{companyId}/branches/{branchId}/branch-managers/{branchManagerId}

**2. Description**

- Show branch manager

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            code
            email
            name
            name_kana
            role
            status
            phone_number
            created_at
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch manager cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchManagerController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchManagerRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create branch manager.
**1. URL**

- POST: {URL}/companies/{companyId}/branches/{branchId}/branch-managers

**2. Description**

-  Create branch manager.
-  Notice: Send email to manager that is created

**3. Last modify**

	// TO DO

**4. Parameter**
      
- code:
    + max:20
    + type_code
    + unique:managers,code,NULL,id,company_id,{$branch->company_id},deleted_at,NULL
    + required
- name:
    + max:40
    + required_with:code,name_kana,email,password,password_confirmation
- name_kana:
    + max:40
    + is_kana
    + fullsize
    + required_with:code,name,email,password,password_confirmation
- email:
    + email
    + max:255
    + required
    + unique:managers,email,NULL,id,company_id,{$branch->company_id},deleted_at,NULL
- phone_number:
    + nullable
    + phone_number
    + max:20
- password:
    + require_trim
    + min:6
    + max:255
    + required
    + type_password
- password_confirmation:
    + require_same:password
       
> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Successfully added person in charge.",
        data:  {
            id
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Error adding administrator.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchManagerController@store
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchManagerRepository
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update branch manager.
**1. URL**

- PUT: {URL}/companies/{companyId}/branches/{branchId}/branch-managers/{branchManagerId}

**2. Description**

-  Update branch manager.

**3. Last modify**

	// TO DO

**4. Parameter**
      
- code:
    + max:20
    + type_code
    + unique:managers,code,$branchManagerId,id,company_id,{$branch->company_id},deleted_at,NULL
    + required
- name:
    + max:40
    + required_with:code,name_kana,email,password,password_confirmation
- name_kana:
    + max:40
    + is_kana
    + fullsize
    + required_with:code,name,email,password,password_confirmation
- email:
    + email
    + max:255
    + required
    + unique:managers,email,$branchManagerId,id,company_id,{$branch->company_id},deleted_at,NULL
- phone_number:
    + nullable
    + phone_number
    + max:20
- password:
    + require_trim
    + min:6
    + max:255
    + required
    + type_password
- password_confirmation:
    + require_same:password
       
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully updated person in charge.",
        data:  []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating the contact person.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchManagerController@update
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchManagerRepository
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete branch manager.
**1. URL**

- DELETE: {URL}/companies/{companyId}/branches/{branchId}/branch-admin/{branchAdminId}

**2. Description**

- Delete branch manager.

**3. Last modify**

	// TO DO

**4. Parameter**

- current_password:
    + required
    + current_password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully deleted person in charge. ",
        data:  []
    }

- 403:
    {
        status_code: 403,
        message: "The current password does not match.",
        errors: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred deleting the contact person.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchManagerController@destroy
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchManagerRepository
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Add branch manager by id
**1. URL**

- POST: {URL}/companies/{companyId}/branches/{branchId}/add-branch-managers/{managerId}

**2. Description**

- Add branch manager by id

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully added person in charge.",
        data:  []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Branch manager cannot be created.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchManagerController@addBranchManagerId
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchManagerRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Detach branch manager by id
**1. URL**

- POST: {URL}/companies/{companyId}/branches/{branchId}/detach-branch-admin/{managerId}

**2. Description**

- Detach manager out branch.

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully deleted the person in charge",
        data:  []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Failed to delete person in charge.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchManagerController@detachBranchManager
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchManagerRepository
    
**7. Table get data**

- branch_manager

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List manager of branch role manager
**1. URL**

- GET: {URL}/companies/{companyId}/branches/{id}/managers

**2. Description**

- List all managers of branch role manager

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:   [
            {
                id
                code
                name
                name_kana
                email
                phone_number
            }
        ]
    }
    
- 500:
    {
        status_code: 500,
        message: "Manager cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ManagerController@listBranchManagers
- Service:
- Repository:
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branch_manager
- managers

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List manager of branch role admin
**1. URL**

- GET: {URL}/companies/{companyId}/branches/{id}/admins

**2. Description**

- List all managers of branch role admin

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:   [
            {
                id
                code
                name
                name_kana
                email
                phone_number
            }
        ]
    }
    
- 500:
    {
        status_code: 500,
        message: "Manager cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ManagerController@listBranchAdmins
- Service:
- Repository:
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branch_manager
- managers

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List all managers of company
**1. URL**

- GET: {URL}/companies/{companyId}/managers

**2. Description**

- List managers of company

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:   [
            {
                id
                code
                name
                name_kana
                email
                phone_number
            }
        ]
    }
    
- 500:
    {
        status_code: 500,
        message: "Manager cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ManagerController@listManagersCompany
- Service:
- Repository:
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- branch_manager
- managers

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Branch prepaid setting
### List prepaid setting
**1. URL**

- GET: {URL}/companies/{companyId}/branches/{branchId}/prepaid-setting

**2. Description**

- Show prepaid setting of branch

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            upper_limit_rate
            upper_limit_total_per_day
            transaction_fee_rate
            upper_limit_prepayment_fee
            consumption_tax_flag
            insurance_fee
            created_at
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Prepaid setting of branch cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchController@showBranchPrepaidSetting
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchPrepaidSettingRepository
    
**7. Table get data**

- branch_prepaid_setting

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update prepaid setting
**1. URL**

- PUT: {URL}/companies/{companyId}/branches/{branchId}/prepaid-setting

**2. Description**

- Update prepaid setting of branch

**3. Last modify**

	// TO DO

**4. Parameter**

- upper_limit_rate:
    + required
    + integer
    + min:1
    + max:company_upper_limit_rate
- upper_limit_total_per_day:
    + nullable
    + integer
    + min:company_lower_limit_per_once
    + max:company_upper_limit_total_per_day
- insurance_fee:
    + required
    + integer
    + min:0
    + max:company_insurance_fee

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update prepaid setting of branch.",
        data: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating the prepaid setting of branch.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchController@updateBranchPrepaidSetting
- Service:
- Repository:
    + App\Repositories\Eloquent\BranchPrepaidSettingRepository
    + App\Repositories\Eloquent\BranchRepository
    
**7. Table get data**

- branch_prepaid_setting

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Company kintai
### List field to map csv
**1. URL**

- GET: {URL}/companies/{companyId}/employee-kintai-columns

**2. Description**

- List column employee kintai

**3. Last modify**

	// TO DO

**4. Parameter**

- type: required

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                name
            }
        ]
    }
    
- 500:
    {
        status_code: 500,
        message: "List columns cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyKintaiController@listColumns
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyKintaiRepository
    
**7. Table get data**

- company_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List company kintai format
**1. URL**

- GET: {URL}/companies/{companyId}/company-kintai-formats

**2. Description**

- List company kintais

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    name
                    type
                    created_at
                    company_kintai_attribute_mapping_count
                    company_kintai_attribute_count
                    type_object: [
                        {
                            id
                            name
                        }
                    ]
                }
            ]
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Company kintai cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyKintaiController@listCompanyKintaiFormat
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyKintaiRepository
    
**7. Table get data**

- company_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create company kintai format
**1. URL**

- POST: {URL}/companies/{companyId}/company-kintai-formats

**2. Description**

- Create employee kintai format

**3. Last modify**

	// TO DO

**4. Parameter**

- name: 
    + required
    + unique:company_kintais,name,NULL,id,company_id,$companyId
    + max:255
- data: 
    + array
    + required
    + not_same_format
    + string_kintai_format
    + in_type_kintai_format
    + kintai_format_type3
    + require_must_kintai_mapping
- type:
    + unique:company_kintais,type,NULL,id,company_id,$companyId
    + required
    + in:CompanyKintai::$type

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Company kintais has been added successfully.",
        data: {
            id
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Create company kintais error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyKintaiController@createCompanyKintaiFormat
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyKintaiRepository
    + App\Repositories\Eloquent\CompanyKintaiAttributeRepository
    
**7. Table get data**

- company_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update company kintai format
**1. URL**

- PUT: {URL}/companies/{companyId}/company-kintai-formats/{companyKintaiId}

**2. Description**

- Update employee kintai format

**3. Last modify**

	// TO DO

**4. Parameter**

- name: 
    + required
    + unique:company_kintais,name,$companyKintaiId,id,company_id,$companyId
    + max:255
- data: 
    + array
    + required
    + not_same_format
    + string_kintai_format
    + in_type_kintai_format
    + kintai_format_type3
    + require_must_kintai_mapping

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Company kintais has been updated successfully.",
        data: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Update company kintais error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyKintaiController@updateCompanyKintaiFormat
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyKintaiRepository
    + App\Repositories\Eloquent\CompanyKintaiAttributeRepository
    
**7. Table get data**

- company_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show company kintai format
**1. URL**

- GET: {URL}/companies/{companyId}/company-kintai-formats/{companyKintaiId}

**2. Description**

-  Show employee kintai format

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            id
            name
            type
            created_at
            type_object: [
                {
                    id
                    name
                }
            ],
            company: {
                id
                code
                name
                name_kana
                zip_code
                address1
                address2
                department_name_first_part
                department_name_middle_part
                department_name_last_part
            },
            company_kintai_attribute: [
                {
                    id
                    company_kintai_id
                    attribute
                    value
                    order
                    text_value
                }
            ]
        }
    }
   
- 500:
    {
        status_code: 500,
        message: "Company kintai cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyKintaiController@showCompanyKintaiFormat
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyKintaiRepository
    
**7. Table get data**

- company_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete company kintai format
**1. URL**

- DELETE: {URL}/companies/{companyId}/company-kintai-formats/{companyKintaiId}

**2. Description**

-   Delete company kintai

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Company kintais has been deleted successfully.",
        data: []
    }
   
- 404:
    {
        status_code: 404,
        message: "Company kintai is not found.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Company kintais delete fail",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyKintaiController@deleteCompanyKintaiFormat
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyKintaiRepository
    
**7. Table get data**

- company_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Check create full company kintai
**1. URL**

- GET: {URL}/companies/{companyId}/check-kintai-patterns

**2. Description**

-   Check kintai pattern

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            can_create  
        }
    }
   
- 404:
    {
        status_code: 404,
        message: "Company kintai is not found.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred while checking company kintai pattern.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyKintaiController@checkKintaiPattern
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyKintaiRepository
    
**7. Table get data**

- company_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Announcement
### List announcements
**1. URL**

- GET: {URL}/announcements

**2. Description**

- List announcements

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable
- status: nullable
- sort_column: nullable
- sort_direction: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    enigma_user_id
                    manager_id
                    company_id
                    title
                    start_time_at
                    end_time_at
                    is_emergency
                    created_at
                    type
                    status
                    enigma_user: {
                        id
                        name
                        role_name
                    },
                    company: {
                        id
                        name
                        is_deleted
                    },
                    manager
                }
            ]
        }
    }

- 500:
    {
        status_code: 500,
        message: "Occur an error while listing announcements.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ AnnouncementController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\AnnouncementRepository
    
**7. Table get data**

- enigma_announcements

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show announcement
**1. URL**

- GET: {URL}/announcements/{id}

**2. Description**

- Show announcement

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            id
            enigma_user_id
            manager_id
            company_id
            title
            start_time_at
            end_time_at
            is_emergency
            created_at
            type
            content
            enigma_user: {
                id
                name
                role_name
            },
            company: {
                id
                name
                is_deleted
            },
            manager
        }
    }

- 404:
    {
        status_code: 404,
        message: "Announcement is not found.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while showing announcement.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ AnnouncementController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\AnnouncementRepository
    
**7. Table get data**

- enigma_announcements

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create a announcement
**1. URL**

- POST: {URL}/announcements

**2. Description**

- Create a announcement

**3. Last modify**

	// TO DO

**4. Parameter**

- title:
    + required
    + max:255
- content:
    + required
- is_emergency: 
    + required
    + boolean
- start_time_at:
    + required
    + date_format:Y-m-d H:i
    + after:now
- end_time_at:
    + required
    + date_format:Y-m-d H:i
    + after:start_time_at

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Successfully create announcement.",
        data: {
            id
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Occur an error while creating announcement.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ AnnouncementController@store
- Service:
- Repository:
    + App\Repositories\Eloquent\AnnouncementRepository
    
**7. Table get data**

- enigma_announcements

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update announcement
**1. URL**

- PUT: {URL}/announcements/{id}

**2. Description**

- Update announcement

**3. Last modify**

	// TO DO

**4. Parameter**

- title:
    + required
    + max:255
- content:
    + required
- is_emergency: 
    + required
    + boolean
- start_time_at:
    + required
    + date_format:Y-m-d H:i
    + after:now
- end_time_at:
    + required
    + date_format:Y-m-d H:i
    + after:start_time_at

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update the announcement.",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while updating announcement.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ AnnouncementController@update
- Service:
- Repository:
    + App\Repositories\Eloquent\AnnouncementRepository
    
**7. Table get data**

- enigma_announcements

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete announcement
**1. URL**

- DELETE: {URL}/announcements/{id}

**2. Description**

- Delete announcement

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully delete the announcement.",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while deleting announcement.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ AnnouncementController@destroy
- Service:
    - Repository:
    + App\Repositories\Eloquent\AnnouncementRepository
    
**7. Table get data**

- enigma_announcements

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Enigma setting
### Show enigma setting
**1. URL**

- GET: {URL}/enigma-setting

**2. Description**

- Show setting

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            id
            consumption_tax
            created_at
        }
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while showing settings.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaSettingController@show
- Service:
    + App\Services\EnigmaSetting\ShowService
- Repository:
    + App\Repositories\Eloquent\EnigmaSettingRepository
    
**7. Table get data**

- enigma_setting

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create and update enigma setting
**1. URL**

- POST: {URL}/enigma-setting

**2. Description**

- Save a enigma setting

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Save settings fee successfully.",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while saving settings.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaSettingController@save
- Service:
    + App\Services\EnigmaSetting\UpdateService
- Repository:
    + App\Repositories\Eloquent\EnigmaSettingRepository
    
**7. Table get data**

- enigma_setting

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Enigma user
### Show info of enigma user
**1. URL**

- GET: {URL}/enigma-users/profile

**2. Description**

- Show profile info of current logined user

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            email
            name
            name_kana
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur error when showing your information.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@showProfile
- Service:
    + App\Services\EnigmaSetting\UpdateService
- Repository:
    + App\Repositories\Eloquent\EnigmaSettingRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update info of enigma user
**1. URL**

- PUT: {URL}/enigma-users/profile

**2. Description**

- Update profile info of enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

- email:
    + email
    + max:255
    + required
    + checkEmailDomain
    + unique:enigma_users,email,$id,id,deleted_at,NULL
- name:
    + max:40
    + required
- name_kana:
    + max:40
    + is_kana
    + fullsize
    + required

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Your information has been updated successfully.",
        data: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur error when updating your information.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@updateProfile
- Service:
    + App\Services\EnigmaUserService
- Repository:
    + App\Repositories\Eloquent\EnigmaUserRepository
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update password of enigma user
**1. URL**

- PUT: {URL}/enigma-users/password

**2. Description**

- Change password of enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

- current_password:
    + require_trim
    + required
    + current_password
- password:
    + require_trim
    + min:6
    + max:255
    + required
    + type_password
    + different:current_password
- password_confirmation:
    +require_same:password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Password has been updated successfully.",
        data: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Password cannot be updated",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@updatePassword
- Service:
- Repository:
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO
	
### List enigma user
**1. URL**

- GET: {URL}/enigma-users

**2. Description**

- List enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    role
                    id
                    email
                    name
                    name_kana
                    role_name
                }
            ]
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while listing enigma admin.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show info of enigma user
**1. URL**

- GET: {URL}/enigma-users/{enigmaUserId}

**2. Description**

- Show info of enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            role
            id
            email
            name
            name_kana
            role_name
        }
    }
    
- 404:
    {
        status_code: 404,
        message: "Enigma admin is not found.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while showing enigma admin.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create enigma user
**1. URL**

- POST: {URL}/enigma-users

**2. Description**

- Create enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

- role:
    + required
    + integer
    + in:EnigmaUser::ROLE_ADMIN, EnigmaUser::ROLE_VIEWER
- email:
    + email
    + max:255
    + required
    + unique:enigma_users,email,NULL,id,deleted_at,NULL
    + checkEmailDomain
- name:
    + max:40
    + required
- name_kana:
    + max:40
    + is_kana
    + fullsize
    + required
- password:
    + require_trim
    + required
    + type_password
    + min:6
    + max:255
- password_confirmation:
    + require_same:password

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Enigma admin has been added successfully.",
        data: {
            id
        }
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Create enigma admin error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@store
- Service:
- Repository:
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update info enigma user
**1. URL**

- PUT: {URL}/enigma-users/{enigmaUserId}

**2. Description**

- Update info enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

- role:
    + required
    + integer
    + in:EnigmaUser::ROLE_ADMIN, EnigmaUser::ROLE_VIEWER
- email:
    + email
    + max:255
    + required
    + unique:enigma_users,email,$enigmaUserId,id,deleted_at,NULL
    + checkEmailDomain
- name:
    + max:40
    + required
- name_kana:
    + max:40
    + is_kana
    + fullsize
    + required
- password:
    + require_trim
    + required
    + type_password
    + min:6
    + max:255
- password_confirmation:
    + require_same:password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Enigma admin has been updated successfully.",
        data: []
    }
    
- 404:
    {
        status_code: 404,
        message: "Enigma admin is not found.",
        errors: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Update enigma admin error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@store
- Service:
    + App\Services\EnigmaUserService
- Repository:
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete enigma user
**1. URL**

- DELETE: {URL}/enigma-users/{enigmaUserId}

**2. Description**

- Delete enigma user

**3. Last modify**

	// TO DO

**4. Parameter**

- current_password:
    + required
    + current_password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Enigma admin has been deleted successfully.",
        data: []
    }
    
- 404:
    {
        status_code: 404,
        message: "Enigma admin is not found.",
        errors: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Enigma admin delete fail.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaUserController@store
- Service:
    + App\Services\EnigmaUserService
- Repository:
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- enigma_users

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Payment request
### Export CSV payment request
**1. URL**

- GET: {URL}/employee-payment-requests/export-csv

**2. Description**

- Export CSV payment request

**3. Last modify**

	// TO DO

**4. Parameter**

- company_code: nullable
- company_name: nullable
- bank_code: nullable
- branch_name: nullable
- employee_name: nullable
- employee_code: nullable
- status: nullable
- request_apply_from: nullable
- request_apply_to: nullable
- transfer_date_from: nullable
- transfer_date_to: nullable
- sort_column: nullable
- sort_direction: nullable

> **5. Response**

```php
- 201:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format employee-prepaid-salary-date('YmdHis').csv)
    
- 500:
    {
        status_code: 500,
        message: "Employee payment request cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PaymentRequestController@exportCsv
- Service:
    + App\Services\PaymentRequests\CsvPaymentRequestService
- Repository:
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update status for payment request
**1. URL**

- PUT: {URL}/employee-payment-requests/{paymentRequestId}/status/{status}

**2. Description**

- Update status for payment request

**3. Last modify**

	// TO DO

**4. Parameter**

- comment

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update status of employee payment request.",
        data: []
    }
    
- 403:
    {
        status_code: 403,
        message: "Permission denied",
        errors: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating status of employee payment request.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PaymentRequestController@updateStatus
- Service:
    + App\Services\PaymentRequests\UpdateStatusPaymentRequestService
    + App\Services\PaymentRequests\CommonService
- Repository:
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    + App\Repositories\Eloquent\EmployeeSalaryRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List payment requests
**1. URL**

- GET: {URL}/employee-payment-requests

**2. Description**

- List payment requests

**3. Last modify**

	// TO DO

**4. Parameter**

- company_code: nullable
- company_name: nullable
- bank_code: nullable
- branch_name: nullable
- employee_name: nullable
- employee_code: nullable
- status: nullable
- request_apply_from: nullable
- request_apply_to: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable

> **5. Response**

```php
- 201:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    payment_request_id
                    applied_at
                    status: {
                        id
                        name
                    },
                    company_name
                    branch_name
                    employee_code
                    employee_name
                    amount_of_salary
                    amount_of_transfer_fee
                    amount_of_transaction_fee
                    amount_of_consumption_tax
                    total_fee
                    can_edit
                }
            ]
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Employee payment request cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PaymentRequestController@index
- Service:
    + App\Services\PaymentRequests\IndexService
- Repository:
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show employee payment request
**1. URL**

- GET: {URL}/employee-payment-requests/{id}

**2. Description**

- Show employee payment request

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 201:
    {
        status_code: 200,
        message: "",
        data: {
            id
            employee_id
            applied_at
            status
            company_name
            branch_name
            employee_code
            employee_name
            amount_of_salary
            amount_of_transfer_fee
            transfer_date
            total_fee
            amount_of_transaction_fee
            status_object: {
                id
                name
            }
        }
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Employee payment request cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PaymentRequestController@show
- Service:
    + App\Services\EmployeePaymentRequestService
- Repository:
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update payment request
**1. URL**

- PUT: {URL}/employee-payment-requests/{id}

**2. Description**

- Update transfer date of payment request

**3. Last modify**

	// TO DO

**4. Parameter**

- transfer_date:
    + required
    + date_format:Y-m-d
    + days_in_between:applied_at,now()

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully updated the transfer date",
        data: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating the transfer date",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PaymentRequestController@update
- Service:
    + App\Services\EmployeePaymentRequestService
- Repository:
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List payment requests of branch
**1. URL**

- GET: {URL}/companies/{companyId}/branch-payment-requests

**2. Description**

-  List payment requests of branch

**3. Last modify**

	// TO DO

**4. Parameter**

- schedule_id: required
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                company_id
                total_applies
                total_employee_prepaid
                total_amount_of_salary
                total_transaction_fee
                total_fee
                total_consumption_tax
                amount_of_transfer_fee
                branch_code
            }
        ]
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Prepayment request cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchPaymentRequestController@index
- Service:
    + App\Services\BranchPaymentRequests\ListPaymentRequestService
    + App\Services\CommonApis\ScheduleSelectionService
- Repository:
    + App\Repositories\Eloquent\BranchPaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show payment request of branch
**1. URL**

- GET: {URL}/companies/{companyId}/branch-payment-requests/{id}

**2. Description**

-  Show payment request of branch

**3. Last modify**

	// TO DO

**4. Parameter**

- employee_status:
- employee_name:
- employee_code:
- employee_name_kana:
- created_at_from:
- created_at_to:
- approved_at_from:
- approved_at_to:
- start_contract_time_from:
- start_contract_time_to:
- end_contract_time_from:
- end_contract_time_to:
- schedule_id:
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
                id
                company_id
                total_applies
                total_employee_prepaid
                total_amount_of_salary
                total_transaction_fee
                total_fee
                total_consumption_tax
                amount_of_transfer_fee
                branch_code
        }
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Employee prepayment request cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BranchPaymentRequestController@show
- Service:
    + App\Services\BranchPaymentRequests\ShowService
- Repository:
    + App\Repositories\Eloquent\BranchPaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List payment requests by company
**1. URL**

- GET: {URL}/company-payment-requests

**2. Description**

-  List payment requests by company

**3. Last modify**

	// TO DO

**4. Parameter**

- company_name: nullable
- company_code: nullable
- status: nullable
- year: nullable
- month: nullable
- sort_column: nullable
- sort_direction: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                company_code
                company_name
                total_register_employees
                total_employee_request
                total_payment_request
                total_fee
            }
        ]
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Prepayment request cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get date for statistic payment request of company
**1. URL**

- GET: {URL}/companies/{id}/date-payment-request-statistics

**2. Description**

-  Get date for statistic payment request of company

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                date
            }
        ]
    }
        
- 500:
    {
        status_code: 500,
        message: "Prepayment request cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestController@getDatePaymentRequestStatistics
- Service:
    + App\Services\PaymentRequestStatisticService
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show statistic payment request of company
**1. URL**

- GET: {URL}/companies/{id}/payment-request-statistics

**2. Description**

-  Show statistic payment request of company

**3. Last modify**

	// TO DO

**4. Parameter**

- year: nullable
- month: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total_apply_employees
            request_nums
            total_salary
            total_transaction_fee
            total_transfer_fee
            total_consumption_tax
            total_request
            total
        }
    }
            
- 500:
    {
        status_code: 500,
        message: "Prepayment request cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestController@showPaymentRequestStatistics
- Service:
    + App\Services\PaymentRequestStatisticService
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get months that exists schedule for prepayment
**1. URL**

- GET: {URL}/company-payment-requests/schedule-months

**2. Description**

-  Get months that exists schedule for prepayment

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                text
                year
                month
            }
        ]
    }
            
- 500:
    {
        status_code: 500,
        message: "Month cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestController@getMonthsOfCompanySchedule
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export batch pdf file company payment request.
**1. URL**

- GET: {URL}/company-payment-requests/{companyId}/export-pdf

**2. Description**

-  Export batch pdf file company payment request.

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    Return file PDF
            
- 500:
    {
        status_code: 500,
        message: "An error occurred while downloading PDF.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestController@exportPdfFile
- Service:
    + App\Services\PaymentRequests\ExportPdfService
    + App\Services\RequestFormService
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    + App\Repositories\Eloquent\EnigmaSettingRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Check if company have previous cycle
**1. URL**

- GET: {URL}/companies/{companyId}/previous-cycle/is-exists

**2. Description**

-  Check if company have previous cycle

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            is_exists_previous_cycle
        }
    }
            
- 500:
    {
        status_code: 500,
        message: "An error occurred while checking whether or not previous cycle exists.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestController@isExistPreviousCycle
- Service:
- Repository:
    + App\Repositories\Eloquent\CompanyPaymentScheduleRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get list month of employee payment request
**1. URL**

- GET: {URL}/company-payment-requests/month-payment-request

**2. Description**

-  Get list month of employee payment request

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  [
            {
                date
                year
                month
            }
        ]
    }
            
- 500:
    {
        status_code: 500,
        message: "Month cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestController@getMonthOfPaymentRequest
- Service:
    + App\Services\PaymentRequestStatisticService
- Repository:
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List company payment request to download
**1. URL**

- GET: {URL}/company-payment-requests/batch-download

**2. Description**

-  List company payment request to download

**3. Last modify**

	// TO DO

**4. Parameter**

- company_ids
- month
- year
- cycle_closing_kintai_day
- per_page: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
                  
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                id
                name
                motnth
                year
            ]
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
                
- 500:
    {
        status_code: 500,
        message: "Company cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestDownloadController@listDownload
- Service:
    + App\Services\PaymentRequests\ListDownloadService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export batch pdf file company payment request.
**1. URL**

- GET: {URL}/company-payment-requests/export-batch-pdf

**2. Description**

- Export batch pdf file company payment request.

**3. Last modify**

	// TO DO

**4. Parameter**

- month
- year
- cycle_closing_kintai_day
- company_download_ids
- company_remove_ids
- per_page: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
                  
> **5. Response**

```php
- 200:
   Return file PDF

- 422:
    {
        status_code: 422,
        message: "Can not output request form of specified month.",
        errors: []
    }
                
- 500:
    {
        status_code: 500,
        message: "An error occurred while downloading PDF.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyPaymentRequestDownloadController@exportBatchPdfFile
- Service:
    + App\Services\PaymentRequests\ExportBatchPdfService
    + App\Services\RequestFormService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\ CompanyPaymentRequestRepository
    + App\Repositories\Eloquent\EnigmaSettingRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Employee
### Show employee payment request
**1. URL**

- GET: {URL}/employees/{id}/payment-requests

**2. Description**

- Show employee payment request

**3. Last modify**

	// TO DO

**4. Parameter**

- schedule_id
                  
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  [
            {
                id
                created_at
                employee_id
                type
            }
        ]
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
                
- 500:
    {
        status_code: 500,
        message: "Employee payment request cannot be showed",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ PaymentRequestController@showEmployeePaymentRequest
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeeRepository
    + App\Repositories\Eloquent\EmployeePaymentRequestRepository
    
**7. Table get data**

- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show employee
**1. URL**

- GET: {URL}/employees/{id}

**2. Description**

- Show detail of employee

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            code
            email
            name
            name_kana
            is_social_insurance
            is_social_insurance_flag
            insurance_fee_schedule
            insurance_policy_schedule
            insurance_policy: {
                id
                name
            },
            insurance_fee
            is_employment_insurance
            phone_number
            branch_id
            company_id
            status
            contract_type: {
                id
                name
            },
            deposit_type: {
                id
                name
            },
            start_contract_time
            end_contract_time
            bank_code
            bank_name_kana
            bank_branch_code
            bank_branch_name_kana
            bank_account_number
            bank_account_holder_kana
            status_object: {
                id
                name
            },
            company_kintais: [
                {
                    id
                    name
                    data: [
                        {
                            id
                            name
                            type
                        }
                    ]
                }
            ],
            stopped_status: {
                reason
                reason_detail
                stopped_at
            },
            stopped_reason: {
                id
                name
            },
            branch: {
                id
                code
                name
                name_kana
            },
            previous_branch_schedule
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
                
- 500:
    {
        status_code: 500,
        message: "Employee cannot be showed",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeeRepository
    + App\Repositories\Eloquent\EmployeeKintaiRepository
    + App\Repositories\Eloquent\CompanyKintaiRepository
    + App\Repositories\Eloquent\EmployeeStatusStopRepository
    
**7. Table get data**

- employees

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List employees kintai by pattern
**1. URL**

- GET: {URL}/employees/{employeeId}/company-kintais/{companyKintaiId}/pattern

**2. Description**

- List employees kintai by pattern

**3. Last modify**

	// TO DO

**4. Parameter**

- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
                  
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    branch_name
                    employee_code
                    employee_name
                    employee_name_kana
                    updated_date
                    total_amount_of_salary
                    created_at
                    employee_kintai_values: [
                        {
                            id
                            value
                            employee_kintai_id
                        }
                    ]
                }
            ]
        }
    }
                
- 500:
    {
        status_code: 500,
        message: "Employee kintai cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeKintaiController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeeRepository
    + App\Repositories\Eloquent\EmployeeKintaiRepository
    + App\Repositories\Eloquent\CompanyKintaiRepository
    
**7. Table get data**

- employee_kintais

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Password
### List employee request change password
**1. URL**

- GET: {URL}/password-managements

**2. Description**

- List employee request change password

**3. Last modify**

	// TO DO

**4. Parameter**

- company_name: nullable
- employee_code: nullable
- employee_name_kana: nullable
- employee_name: nullable
- apply_date_to: nullable
- apply_date_from: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
                  
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: {
                id
                employee_id
                created_at
                company_name
                employee_code
                employee_name
                employee_name_kana
                employee_phone
                reset_at
            }
        }
    }
                
- 500:
    {
        status_code: 500,
        message: "An error occurred while reset employee password.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ForgotPasswordController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\ForgotPasswordRepository
    
**7. Table get data**

- employee_forgot_passwords

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show detail employee forgot password
**1. URL**

- GET: {URL}/password-managements/{id}

**2. Description**

- Show detail employee forgot password

**3. Last modify**

	// TO DO

**4. Parameter**
                  
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            updated_at
            employee_phone
            reset_at
        }
    }
                
- 500:
    {
        status_code: 500,
        message: "Reset password cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ForgotPasswordController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\ForgotPasswordRepository
    
**7. Table get data**

- employee_forgot_passwords

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Reset employee password
**1. URL**

- PUT: {URL}/password-managements/{id}

**2. Description**

- Reset employee password

**3. Last modify**

	// TO DO

**4. Parameter**
                  
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Reset password success",
        data:  []
    }
                
- 500:
    {
        status_code: 500,
        message: "Token is invalid",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ForgotPasswordController@update
- Service:
- Repository:
    + App\Repositories\Eloquent\ForgotPasswordRepository
    + App\Repositories\Eloquent\EmployeeRepository
    
**7. Table get data**

- employee_forgot_passwords

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Activity log
### List activity log
**1. URL**

- GET: {URL}/activity-logs

**2. Description**

- List activity log

**3. Last modify**

	// TO DO

**4. Parameter**
      
- start_date            
- end_date
- per_page
              
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  [
            {
                id
                company_id
                subject_id
                subject_type
                causer_id
                causer_type
                action
                action_id
                properties
                created_at
                causer_name
                company_name
            }
        ]
    }
                
- 500:
    {
        status_code: 500,
        message: "Occur an error while listing activity logs.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ActivityLogController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\ActivityLogRepository
    
**7. Table get data**

- activity_logs

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Setting banks
### List bank settings
**1. URL**

- GET: {URL}/banks/settings

**2. Description**

- List bank settings

**3. Last modify**

	// TO DO

**4. Parameter**
      
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  [
            {
                id
                bank_type
                is_send_bizfax
                consecutive_number
            }
        ]
    }
                
- 500:
    {
        status_code: 500,
        message: "Setting bank cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ SettingBankController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\SettingBankRepository
    
**7. Table get data**

- company_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update setting banks
**1. URL**

- POST: {URL}/banks/settings

**2. Description**

- Update setting banks

**3. Last modify**

	// TO DO

**4. Parameter**
      
- is_send_bizfax:
    + required
    + boolean
- consecutive_number:
    + required
    + min:0
    + integer
- bank_type:
    + required
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update setting banks.",
        data:  []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
                    
- 500:
    {
        status_code: 500,
        message: "An error occurred while updating banks.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ SettingBankController@update
- Service:
- Repository:
    + App\Repositories\Eloquent\SettingBankRepository
    
**7. Table get data**

- company_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Employee pattern
### Check create full employee pattern
**1. URL**

- GET: {URL}/companies/{companyId}/check-employee-patterns

**2. Description**

- Check create full employee pattern

**3. Last modify**

	// TO DO

**4. Parameter**
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            can_create
        }
    }
      
- 500:
    {
        status_code: 500,
        message: "An error occurred while checking employee pattern.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeePatternController@checkEmployeePattern
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeePatternRepository
    
**7. Table get data**

- employee_patterns

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List field to map csv
**1. URL**

- GET: {URL}/companies/{companyId}/employee-pattern-columns

**2. Description**

- List column employee pattern

**3. Last modify**

	// TO DO

**4. Parameter**
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  [
            {
                id
                name
            }
        ]
    }
      
- 500:
    {
        status_code: 500,
        message: "List columns cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeePatternController@listEmployeePatternColumns
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeePatternRepository
    
**7. Table get data**


**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List employee pattern
**1. URL**

- GET: {URL}/companies/{companyId}/employee-patterns

**2. Description**

- List column employee pattern

**3. Last modify**

	// TO DO

**4. Parameter**
    
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    name
                    created_at
                    employee_pattern_attribute_mapping_count
                    employee_pattern_attribute_count
                }
            ]
        }
    }
      
- 500:
    {
        status_code: 500,
        message: "Employee pattern cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeePatternController@listEmployeePatternColumns
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeePatternRepository
    
**7. Table get data**

- employee_patterns
- employee_pattern_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show employee pattern
**1. URL**

- GET: {URL}/companies/{companyId}/employee-patterns/{id}

**2. Description**

- Show employee pattern

**3. Last modify**

	// TO DO

**4. Parameter**


> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            name
            created_at
            company: {
                id
                code
                name
                name_kana
                zip_code
                address1
                address2
                department_name_first_part
                department_name_middle_part
                department_name_last_part
            },
            employee_pattern_attribute: [
                {
                    id
                    employee_pattern_id
                    attribute
                    value
                    order
                    text_value
                }
            ]
        }
    }
      
- 500:
    {
        status_code: 500,
        message: "Employee pattern cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeePatternController@index
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeePatternRepository
    
**7. Table get data**

- employee_patterns
- employee_pattern_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create employee pattern
**1. URL**

- POST: {URL}/companies/{companyId}/employee-patterns

**2. Description**

- Create employee pattern

**3. Last modify**

	// TO DO

**4. Parameter**

- name:
    + array
    + required
    + not_same_format
    + string_pattern_format
    + in_employee_pattern_format
    + require_must_employee_pattern_mapping
- data:
    + required
    + unique:employee_patterns,name,NULL,id,company_id,$companyId
    + max:255

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Employee pattern has been added successfully.",
        data:  {
            id
        }
    }
      
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Create employee pattern error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeePatternController@store
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeePatternRepository
    + App\Repositories\Eloquent\EmployeePatternAttributeRepository
    
**7. Table get data**

- employee_patterns
- employee_pattern_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update employee pattern
**1. URL**

- PUT: {URL}/companies/{companyId}/employee-patterns/{id}

**2. Description**

- Update employee pattern

**3. Last modify**

	// TO DO

**4. Parameter**

- name:
    + array
    + required
    + not_same_format
    + string_pattern_format
    + in_employee_pattern_format
    + require_must_employee_pattern_mapping
- data:
    + required
    + unique:employee_patterns,name,$id,id,company_id,$companyId
    + max:255

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Employee pattern has been updated successfully.",
        data:  []
    }
      
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Update employee pattern error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeePatternController@update
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeePatternRepository
    + App\Repositories\Eloquent\EmployeePatternAttributeRepository
    
**7. Table get data**

- employee_patterns
- employee_pattern_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete employee pattern
**1. URL**

- DELETE: {URL}/companies/{companyId}/employee-patterns/{id}

**2. Description**

- Delete employee pattern

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Employee pattern has been deleted successfully.",
        data:  []
    }
      
- 404:
    {
        status_code: 404,
        message: "Employee pattern not found.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Employee pattern delete fail",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeePatternController@destroy
- Service:
- Repository:
    + App\Repositories\Eloquent\EmployeePatternRepository
    
**7. Table get data**

- employee_patterns
- employee_pattern_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Employee lock
### List employee lock
**1. URL**

- GET: {URL}/employee-lockes

**2. Description**

- List employees who are locked

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable
- name_kana: nullable
- code: nullable
- name: nullable
- company_name: nullable
- branch_name: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    code
                    name
                    name_kana
                    status
                    company_id
                    branch_id
                    created_at
                    last_login_failed
                    status_object: {
                        id
                        name
                    },
                    branch: {
                        id
                        code
                        name
                        name_kana
                    },
                    company: {
                        id
                        code
                        name
                    }
                }
            ]
        }
    }
     
- 500:
    {
        status_code: 500,
        message: "Employee cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeLockController@index
- Service:
    + App\Services\EmployeeLockService
- Repository:
    + App\Repositories\Eloquent\EmployeeRepository
    
**7. Table get data**

- employees

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Unlock employee
**1. URL**

- DELETE: {URL}/employee-lockes/{employee}/unlock

**2. Description**

- Unlock employee who are locked before

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "The employee has been unlocked successfully",
        data:  []
    }
     
- 500:
    {
        status_code: 500,
        message: "The employee unlock failed",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeLockController@unlockEmployee
- Service:
    + App\Services\EmployeeLockService
- Repository:
    + App\Repositories\Eloquent\EmployeeRepository
    
**7. Table get data**

- employees

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Enigma information
### Get enigma information
**1. URL**

- GET: {URL}/enigma-info

**2. Description**

- Show enigma info

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            company_name
            address1
            address2
            phone_number
            bank_name
            bank_branch_name
            bank_account_type
            bank_account_number
            bank_account_name
            bank_account_type_object: {
                id
                name
            }
        }
    }
     
- 500:
    {
        status_code: 500,
        message: "Setting information of enigma company and transfer destination financial institution cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaInfoController@show
- Service:
- Repository:
    + App\Repositories\Eloquent\EnigmaInfoRepository
    
**7. Table get data**

- enigma_info

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create and update enigma info
**1. URL**

- POST: {URL}/enigma-info

**2. Description**

- Save enigma info

**3. Last modify**

	// TO DO

**4. Parameter**

- company_name:
    + required
    + max:40
- address1:
    + required
    + max:255
- address2:
    + max:255
- phone_number:
    + required
    + phone_number
    + max:20
- bank_name:
    + required
    + max:255
- bank_branch_name:
    + required
    + max:255
- bank_account_type:
    + required
    + in:EnigmaInfo::BANK_TYPE_NORMAL,EnigmaInfo::BANK_TYPE_CURRENT,EnigmaInfo::BANK_TYPE_SAVING
- bank_account_number:
    + required
    + numeric
    + digits_between:7,7
- bank_account_name:
    + required
    + max:255
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Setting information of enigma company and transfer destination financial institution has been updated successfully.",
        data:  []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
         
- 500:
    {
        status_code: 500,
        message: "Setting information of enigma company and transfer destination financial institution cannot be updated.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaInfoController@save
- Service:
- Repository:
    + App\Repositories\Eloquent\EnigmaInfoRepository
    
**7. Table get data**

- enigma_info

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List bank account types
**1. URL**

- GET: {URL}/bank-acount-types

**2. Description**

- List bank account types

**3. Last modify**

	// TO DO

**4. Parameter**
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  [
            {
                id
                name
            }
        ]
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaInfoController@getBankAccountTypes
- Service:
- Repository:
    
**7. Table get data**


**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Manager
### Deactivate MFA
**1. URL**

- PUT: {URL}/managers/{id}/deactivate-mfa-device

**2. Description**

- Deactivate authentication MFA

**3. Last modify**

	// TO DO

**4. Parameter**
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Reset authentication (MFA) has been successfully completed.",
        data:  []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
         
- 500:
    {
        status_code: 500,
        message: "An error occurred while resetting authentication (MFA).",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ManagerController@deactivateMFA
- Service:
    + App\Services\ManagerService
- Repository:
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- managers

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List all managers
**1. URL**

- GET: {URL}/managers

**2. Description**

- List all managers

**3. Last modify**

	// TO DO

**4. Parameter**

- name: nullable
- name_kana: nullable
- code: nullable
- company_id: nullable
- company_code: nullable
- email: nullable
- phone_number: nullable
- created_at: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
    
> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    company_code
                    company_id
                    code
                    email
                    name
                    name_kana
                    phone_number
                    enable_two_factor
                    created_at
                    company: {
                        id
                        code
                        name
                        name_kana
                    }
                }
            ]
        }
    }
  
- 500:
    {
        status_code: 500,
        message: "Manager cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ManagerController@index
- Service:
    + App\Services\ManagerService
- Repository:
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- managers
- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show detail manager
**1. URL**

- GET: {URL}/managers/{managerId}

**2. Description**

- Show detail manager

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            company_code
            company_id
            code
            email
            name
            name_kana
            phone_number
            enable_two_factor
            created_at
            company: {
                id
                code
                name
                name_kana
            }
        }
    }
         
- 500:
    {
        status_code: 500,
        message: "Manager cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ManagerController@index
- Service:
    + App\Services\ManagerService
- Repository:
    + App\Repositories\Eloquent\ManagerRepository
    
**7. Table get data**

- managers
- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Report files
### List report files
**1. URL**

- GET: {URL}/request-form

**2. Description**

- List request form output history

**3. Last modify**

	// TO DO

**4. Parameter**

- company_code: nullable
- from_created_date: nullable
- to_created_date: nullable
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    enigma_user_id
                    filename
                    request_num
                    created_at
                    company_code
                    enigma_user: {
                        id
                        name
                        role_name
                    }
                }
            ]
        }
    }
         
- 500:
    {
        status_code: 500,
        message: "Occur an error while listing request form output history.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ RequestFormController@index
- Service:
    + App\Services\RequestFormService
- Repository:
    + App\Repositories\Eloquent\RequestFormRepository
    
**7. Table get data**

- request_forms

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Download report file
**1. URL**

- GET: {URL}/request-form/{id}/download

**2. Description**

-  Download request output

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    Return file PDF
         
- 500:
    {
        status_code: 500,
        message: "File cannot be downloaded.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ RequestFormController@download
- Service:
    + App\Services\RequestFormService
- Repository:
    + App\Repositories\Eloquent\RequestFormRepository
    
**7. Table get data**

- request_forms

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Notification
### List notification
**1. URL**

- GET: {URL}/notifications

**2. Description**

-  List notification

**3. Last modify**

	// TO DO

**4. Parameter**

- sort_column: nullable
- sort_direction: nullable
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    enigma_user_id
                    title
                    content
                    send_date
                    is_send_all
                    status
                    created_at
                    can_update
                    enigma_user
                    company: [
                        {
                            id
                            name
                            code
                            is_deleted
                        }
                    ]
                }
            ]
        }
    }
         
- 500:
    {
        status_code: 500,
        message: "Occur an error while listing notifications.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ NotificationController@index
- Service:
    + App\Services\NotificationService
- Repository:
    + App\Repositories\Eloquent\NotificationRepository
    
**7. Table get data**

- notifications

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show notification
**1. URL**

- GET: {URL}/notifications/{id}

**2. Description**

-  Show notification

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            enigma_user_id
            title
            content
            send_date
            is_send_all
            status
            created_at
            can_update
            enigma_user: {
                id
                name
                role_name
            },
            company: [
                {
                    id
                    name
                    code
                    is_deleted
                }
            ]
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Occur an error while showing notification.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ NotificationController@show
- Service:
    + App\Services\NotificationService
- Repository:
    + App\Repositories\Eloquent\NotificationRepository
    
**7. Table get data**

- notifications

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create notification
**1. URL**

- POST: {URL}/notifications

**2. Description**

-  Create a notification

**3. Last modify**

	// TO DO

**4. Parameter**

-  title:
    + required
    + max:255
-  content:
    + required
-  send_date:
    + required
    + date_format:Y-m-d H:i
    + after:now
-  is_send_all:
    + required
    + boolean
-  companies

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Successfully create notification.",
        data:  {
            id
        }
    }
         
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
         
- 500:
    {
        status_code: 500,
        message: "Occur an error while creating notification.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ NotificationController@store
- Service:
    + App\Services\NotificationService
- Repository:
    + App\Repositories\Eloquent\NotificationRepository
    
**7. Table get data**

- notifications

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update notification
**1. URL**

- PUT: {URL}/notifications/{id}

**2. Description**

-  Update notification

**3. Last modify**

	// TO DO

**4. Parameter**

-  title:
    + required
    + max:255
-  content:
    + required
-  send_date:
    + required
    + date_format:Y-m-d H:i
    + after:now
-  is_send_all:
    + required
    + boolean
-  companies

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully update the notification.",
        data: []
    }
         
- 403:
    {
        status_code: 403,
        message: "The notification was sent. Can't to update.",
        errors: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
         
- 500:
    {
        status_code: 500,
        message: "Occur an error while updating notification.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ NotificationController@update
- Service:
    + App\Services\NotificationService
- Repository:
    + App\Repositories\Eloquent\NotificationRepository
    
**7. Table get data**

- notifications

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete notification
**1. URL**

- DELETE: {URL}/notifications/{id}

**2. Description**

- Delete notification

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Successfully delete the notification.",
        data: []
    }
         
- 500:
    {
        status_code: 500,
        message: "Occur an error while deleting notification.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ NotificationController@destroy
- Service:
    + App\Services\NotificationService
- Repository:
    + App\Repositories\Eloquent\NotificationRepository
    
**7. Table get data**

- notifications

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Employee deduction
### Check create full employee deduction
**1. URL**

- GET: {URL}/companies/{companyId}/check-employee-deductions

**2. Description**

- Check create full employee deduction

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            can_create
        }
    }
         
- 500:
    {
        status_code: 500,
        message: "An error occurred while checking employee deduction.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeDeductionController@checkEmployeeDeduction
- Service:
    + App\Services\EmployeeDeductionService
- Repository:
    + App\Repositories\Eloquent\EmployeeDeductionRepository
    
**7. Table get data**

- employee_deductions

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List field to map csv
**1. URL**

- GET: {URL}/companies/{companyId}/employee-deduction-columns

**2. Description**

- List column employee deduction

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 2010
        message: "",
        data:  [
            {
                id
                name
            }
        ]
    }
         
- 500:
    {
        status_code: 500,
        message: "List columns cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeDeductionController@listEmployeeDeductionColumns
- Service:
    + App\Services\EmployeeDeductionService
- Repository:
    
**7. Table get data**


**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List employee deduction
**1. URL**

- GET: {URL}/companies/{companyId}/employee-deductions

**2. Description**

- List employee deduction

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    company_id
                    name
                    created_at
                    updated_at
                    employee_deduction_attributes_count
                }
            ]
        }
    }
         
- 500:
    {
        status_code: 500,
        message: "Employee deduction cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeDeductionController@index
- Service:
    + App\Services\EmployeeDeductionService
- Repository:
    + App\Repositories\Eloquent\EmployeeDeductionRepository
    
**7. Table get data**

- employee_deductions

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show employee deduction
**1. URL**

- GET: {URL}/companies/{companyId}/employee-deductions/{id}

**2. Description**

- Show employee deduction

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    company_id
                    name
                    created_at
                    updated_at
                    employee_deduction_attributes_count
                }
            ]
        }
    }
         
- 500:
    {
        status_code: 500,
        message: "Employee deduction cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeDeductionController@show
- Service:
    + App\Services\EmployeeDeductionService
- Repository:
    + App\Repositories\Eloquent\EmployeeDeductionRepository
    
**7. Table get data**

- employee_deductions
- employee_deduction_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create employee deduction
**1. URL**

- POST: {URL}/companies/{companyId}/employee-deductions

**2. Description**

- Create employee deduction

**3. Last modify**

	// TO DO

**4. Parameter**

- name: 
    + required
    + max:255
- data: 
    + array
    + required
    + employee_deduction_mapping_key
    + employee_deduction_duplicate

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Employee deduction has been added successfully.",
        data:  {
            id
        }
    }

- 403:
    {
        status_code: 403,
        message: "You already register employee deduction fully, Create fail.",
        errors: []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    } 
            
- 500:
    {
        status_code: 500,
        message: "Create employee deduction error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeDeductionController@store
- Service:
    + App\Services\EmployeeDeductionService
- Repository:
    + App\Repositories\Eloquent\EmployeeDeductionRepository
    + App\Repositories\Eloquent\EmployeeDeductionAttributeRepository
    
**7. Table get data**

- employee_deductions
- employee_deduction_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update employee deduction
**1. URL**

- PUT: {URL}/companies/{companyId}/employee-deductions/{id}

**2. Description**

- Update employee deduction

**3. Last modify**

	// TO DO

**4. Parameter**

- name: 
    + required
    + max:255
- data: 
    + array
    + required
    + employee_deduction_mapping_key
    + employee_deduction_duplicate

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Employee deduction has been updated successfully.",
        data:  []
    }
    
- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    } 
            
- 500:
    {
        status_code: 500,
        message: "Update employee deduction error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeDeductionController@update
- Service:
    + App\Services\EmployeeDeductionService
- Repository:
    + App\Repositories\Eloquent\EmployeeDeductionRepository
    + App\Repositories\Eloquent\EmployeeDeductionAttributeRepository
    
**7. Table get data**

- employee_deductions
- employee_deduction_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete employee deduction
**1. URL**

- DELETE: {URL}/companies/{companyId}/employee-deductions/{id}

**2. Description**

- Delete employee deduction

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Employee deduction has been deleted successfully.",
        data:  []
    }
       
- 500:
    {
        status_code: 500,
        message: "Employee deduction delete fail.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EmployeeDeductionController@destroy
- Service:
    + App\Services\EmployeeDeductionService
- Repository:
    + App\Repositories\Eloquent\EmployeeDeductionRepository
    
**7. Table get data**

- employee_deductions
- employee_deduction_attributes

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Company admin
### List company admin
**1. URL**

- GET: {URL}/companies/{companyId}/company-admins

**2. Description**

- List company admin

**3. Last modify**

	// TO DO

**4. Parameter**

- code: nullable
- name: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    id
                    company_id
                    company_code
                    code
                    email
                    name
                    name_kana
                    phone_number
                    created_at
                }
            ]
        }
    }
       
- 500:
    {
        status_code: 500,
        message: "Occur an error while listing company admin.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyAdminController@index
- Service:
    + App\Services\CompanyAdminService
- Repository:
    + App\Repositories\Eloquent\CompanyAdminRepository
    
**7. Table get data**

- managers
- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show detail company admin
**1. URL**

- GET: {URL}/companies/{companyId}/company-admins/{companyAdminId}

**2. Description**

- Show detail company admin

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data:  {
            id
            code
            company_id
            company_code
            name
            name_kana
            email
            phone_number
            created_at
        }
    }

- 404:   
   {
       status_code: 404,
       message: "Manager is not found.",
       errors: []
   }
       
- 500:
    {
        status_code: 500,
        message: "Manager cannotbe showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyAdminController@show
- Service:
    + App\Services\CompanyAdminService
- Repository:
    + App\Repositories\Eloquent\CompanyAdminRepository
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- managers
- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create company admin
**1. URL**

- POST: {URL}/companies/{companyId}/company-admins

**2. Description**

- Register a company admin

**3. Last modify**

	// TO DO

**4. Parameter**

- email:
    + required
    + email
    + unique:managers,email,NULL,id,company_id,$companyId,deleted_at,NULL
    + max:255
- name:
    + required
    + max:40
- name_kana:
    + required
    + is_kana
    + max:40
- code:
    + required
    + type_code
    + unique:managers,code,NULL,id,company_id,$companyId,deleted_at,NULL
    + max:255
- phone_number:
    + nullable
    + phone_number
    + max:20
- password:
    + require_trim
    + required
    + type_password
    + min:6
    + max:255
- password_confirm:
    + require_same:password

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Company admin has been added successfully.",
        data:  {
            id
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
       
- 500:
    {
        status_code: 500,
        message: "Create company admin error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyAdminController@store
- Service:
    + App\Services\CompanyAdminService
- Repository:
    + App\Repositories\Eloquent\CompanyAdminRepository
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- managers
- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update company admin
**1. URL**

- PUT: {URL}/companies/{companyId}/company-admins/{companyAdminId}

**2. Description**

- Update a company admin

**3. Last modify**

	// TO DO

**4. Parameter**

- email:
    + required
    + email
    + unique:managers,email,$companyAdminId,id,company_id,$companyId,deleted_at,NULL
    + max:255
- name:
    + required
    + max:40
- name_kana:
    + required
    + is_kana
    + max:40
- code:
    + required
    + type_code
    + unique:managers,code,$companyAdminId,id,company_id,$companyId,deleted_at,NULL
    + max:255
- phone_number:
    + nullable
    + phone_number
    + max:20
- password:
    + require_trim
    + required
    + type_password
    + min:6
    + max:255
- password_confirm:
    + require_same:password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Company admin has been updated successfully.",
        data: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
       
- 500:
    {
        status_code: 500,
        message: "Update company admin error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyAdminController@update
- Service:
    + App\Services\CompanyAdminService
- Repository:
    + App\Repositories\Eloquent\CompanyAdminRepository
    + App\Repositories\Eloquent\CompanyRepository
    
**7. Table get data**

- managers
- companies

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete company admin
**1. URL**

- DELETE: {URL}/companies/{companyId}/company-admins/{companyAdminId}

**2. Description**

- Delete company admin

**3. Last modify**

	// TO DO

**4. Parameter**

- current_password:
    + required
    + current_password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Company admin has been deleted successfully.",
        data: []
    }

- 403:
    {
        status_code: 403,
        message: "Since there is only 1 company admin left now so you can not delete company admin anymore.",
        errors: []
    }
       
- 500:
    {
        status_code: 500,
        message: "Company admin delete fail",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ CompanyAdminController@update
- Service:
    + App\Services\CompanyAdminService
- Repository:
    + App\Repositories\Eloquent\CompanyAdminRepository
    
**7. Table get data**

- managers

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Report
### List employee of companies
**1. URL**

- GET: {URL}/reports/employee-of-companies

**2. Description**

- List employees companies report

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    company_name
                    number_of_employees
                    using
                    stop_service
                    number_of_active_employees
                    number_of_new_registrations
                    number_of_applicants
                }
            ]
        }
    }

- 500:
    {
        status_code: 500,
        message: "Employee report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@listEmployeeCompanies
- Service:
    + App\Services\Report\ReportEmployeeService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employees

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export csv employee of companies
**1. URL**

- GET: {URL}/reports/employee-of-companies/export-csv

**2. Description**

- Export csv employee of companies

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format report-employee-of-companies_date('Ymd').csv)

- 500:
    {
        status_code: 500,
        message: "Employee report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@exportEmployeeCompanyCsv
- Service:
    + App\Services\Report\ReportEmployeeService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employees

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List employee everyday
**1. URL**

- GET: {URL}/reports/employee-everyday

**2. Description**

- List employees everyday report

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    date
                    number_of_active_employees
                    number_of_new_registrations
                    number_of_applicants
                }
            ]
        }
    }
- 500:
    {
        status_code: 500,
        message: "Employee report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@listEmployeeEveryday
- Service:
    + App\Services\Report\ReportEmployeeService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employees

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export csv employee everyday
**1. URL**

- GET: {URL}/reports/employee-everyday/export-csv

**2. Description**

- Export csv employee everyday

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format report-employee-everyday-date('Ymd').csv)

- 500:
    {
        status_code: 500,
        message: "Employee report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@exportEmployeeEverydayCsv
- Service:
    + App\Services\Report\ReportEmployeeService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employees

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List payment request of companies
**1. URL**

- GET: {URL}/reports/payment-request-of-companies

**2. Description**

- List payment request companies report

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    company_name
                    number_of_active_employees
                    number_of_applicants
                    number_of_applications
                    application_amount_of_money
                    sales
                    application_rate
                    average_times
                    average_amount_of_money
                }
            ]
        }
    }
- 500:
    {
        status_code: 500,
        message: "Payment request report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@listPaymentRequestCompanies
- Service:
    + App\Services\Report\ReportPaymentRequestService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export csv payment request companies
**1. URL**

- GET: {URL}/reports/payment-request-of-companies/export-csv

**2. Description**

- Export csv payment request companies

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format report-payment-request-companies-date('Ymd').csv)

- 500:
    {
        status_code: 500,
        message: "Payment request report report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@exportPaymentRequestCompanyCsv
- Service:
    + App\Services\Report\ReportPaymentRequestService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO
	
### List payment request everyday
**1. URL**

- GET: {URL}/reports/payment-request-everyday

**2. Description**

- List payment request everyday report

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    date
                    number_of_active_employees
                    number_of_applicants
                    number_of_applications
                    application_amount_of_money
                    sales
                    average_times
                    average_amount_of_money
                }
            ]
        }
    }
- 500:
    {
        status_code: 500,
        message: "Payment request report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@listPaymentRequestEveryday
- Service:
    + App\Services\Report\ReportPaymentRequestService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export csv payment request everyday
**1. URL**

- GET: {URL}/reports/payment-request-everyday/export-csv

**2. Description**

- Export csv payment request everyday

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format report-payment-request-everyday-date('Ymd').csv)

- 500:
    {
        status_code: 500,
        message: "Payment request report report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@exportPaymentRequestEverydayCsv
- Service:
    + App\Services\Report\ReportPaymentRequestService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_payment_requests

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List login history of companies
**1. URL**

- GET: {URL}/reports/login-history-of-companies

**2. Description**

- List payment request everyday report

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    company_name
                    number_of_logins
                    pc
                    fp
                    sp
                    android
                    ios
                }
            ]
        }
    }
- 500:
    {
        status_code: 500,
        message: "Login history report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@listLoginHistoryCompanies
- Service:
    + App\Services\Report\ReportLoginHistoryService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_login_histories

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export csv login history companies
**1. URL**

- GET: {URL}/reports/login-history-of-companies/export-csv

**2. Description**

- Export csv login history companies

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format report-login-of-company-date('Ymd').csv)

- 500:
    {
        status_code: 500,
        message: "Payment request report report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@exportLoginHistoryCompanyCsv
- Service:
    + App\Services\Report\ReportLoginHistoryService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_login_histories

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List login history everyday
**1. URL**

- GET: {URL}/reports/login-history-everyday

**2. Description**

- List payment request everyday report

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- per_page: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    date
                    number_of_logins
                    pc
                    fp
                    sp
                    android
                    ios
                }
            ]
        }
    }
- 500:
    {
        status_code: 500,
        message: "Login history report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@listLoginHistoryEveryday
- Service:
    + App\Services\Report\ReportLoginHistoryService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_login_histories

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Export csv login history everyday
**1. URL**

- GET: {URL}/reports/login-history-everyday/export-csv

**2. Description**

- Export csv login history everyday

**3. Last modify**

	// TO DO

**4. Parameter**

- time_periods: nullable
- date_from: nullable
- date_to: nullable
- sort_column: nullable
- sort_direction: nullable
- company_id: nullable

> **5. Response**

```php
- 200:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format report-login-everyday-date('Ymd').csv)

- 500:
    {
        status_code: 500,
        message: "Payment request report report cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ ReportController@exportLoginHistoryEverydayCsv
- Service:
    + App\Services\Report\ReportLoginHistoryService
    + App\Services\Report\CommonService
- Repository:
    + App\Repositories\Eloquent\CompanyRepository
    + App\Repositories\Eloquent\GenerateSeriesRepository
    
**7. Table get data**

- companies
- employee_login_histories

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Enigma bank settings
### List enigma bank settings
**1. URL**

- GET: {URL}/enigma-bank-settings

**2. Description**

- List enigma bank settings

**3. Last modify**

	// TO DO

**4. Parameter**

- sort_column: nullable
- sort_direction: nullable
- bank_code: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: [
            {
                id
                bank_code
                bellow_tfee_same_bank
                over_tfee_same_bank
                bellow_tfee_diff_bank
                over_tfee_diff_bank
                created_at
                is_used
            }
        ]
    }
    
- 500:
    {
        status_code: 500,
        message: "Enigma bank setting cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaBankSettingController@index
- Service:
    + App\Services\EnigmaBankSettings\ListService
- Repository:
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    
**7. Table get data**

- enigma_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show a enigma bank setting
**1. URL**

- GET: {URL}/enigma-bank-settings/{id}

**2. Description**

- Show a enigma bank setting

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            id
            bank_code
            bellow_tfee_same_bank
            over_tfee_same_bank
            bellow_tfee_diff_bank
            over_tfee_diff_bank
            created_at
            is_used
        }
    }
    
- 500:
    {
        status_code: 500,
        message: "Enigma bank setting cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaBankSettingController@show
- Service:
    + App\Services\EnigmaBankSettings\ShowService
- Repository:
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    
**7. Table get data**

- enigma_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Create a enigma bank setting
**1. URL**

- POST: {URL}/enigma-bank-settings

**2. Description**

- Create a enigma bank setting

**3. Last modify**

	// TO DO

**4. Parameter**

- bank_code:
    + required
    + numeric
    + digits_between:4,4
    + unique:enigma_bank_settings,bank_code,NULL,id,deleted_at,NULL
- bellow_tfee_same_bank:
    + required
    + integer
    + between:0,10000
- over_tfee_same_bank:
    + required
    + integer
    + between:0,10000
- bellow_tfee_diff_bank:
    + required
    + integer
    + between:0,10000
- over_tfee_diff_bank:
    + required
    + integer
    + between:0,10000

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "Enigma bank setting has been added successfully.",
        data: {
            id
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Create enigma bank setting error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaBankSettingController@store
- Service:
    + App\Services\EnigmaBankSettings\StoreService
- Repository:
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    
**7. Table get data**

- enigma_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update a enigma bank setting
**1. URL**

- PUT: {URL}/enigma-bank-settings/{enigmaBankSettingId}

**2. Description**

- Update a enigma bank setting

**3. Last modify**

	// TO DO

**4. Parameter**

- bank_code:
    + required
    + numeric
    + digits_between:4,4
    + unique:enigma_bank_settings,bank_code,$enigmaBankSettingId,id,deleted_at,NULL
- bellow_tfee_same_bank:
    + required
    + integer
    + between:0,10000
- over_tfee_same_bank:
    + required
    + integer
    + between:0,10000
- bellow_tfee_diff_bank:
    + required
    + integer
    + between:0,10000
- over_tfee_diff_bank:
    + required
    + integer
    + between:0,10000

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Enigma bank setting has been updated successfully.",
        data: []
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Update enigma bank setting error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaBankSettingController@update
- Service:
    + App\Services\EnigmaBankSettings\UpdateSerive
- Repository:
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    
**7. Table get data**

- enigma_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Destroy a enigma bank setting
**1. URL**

- DELETE: {URL}/enigma-bank-settings/{enigmaBankSettingId}

**2. Description**

- Destroy a enigma bank setting

**3. Last modify**

	// TO DO

**4. Parameter**

- current_password:
    + required
    + current_password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Enigma bank setting has been deleted successfully.",
        data: []
    }

- 403:
    {
        status_code: 403,
        message: "Bank code is being registered in system. So can not delete or edit.",
        errors: []
    }
        
- 500:
    {
        status_code: 500,
        message: "Enigma bank setting delete fail.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ EnigmaBankSettingController@update
- Service:
    + App\Services\EnigmaBankSettings\DestroyService
- Repository:
    + App\Repositories\Eloquent\EnigmaBankSettingRepository
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- enigma_bank_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Setting logs
### Get list enigma bank setting logs
**1. URL**

- GET: {URL}/enigma-bank-setting-logs

**2. Description**

- List enigma bank setting logs

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable
- sort_column: nullable
- sort_direction: nullable
- created_at: nullable
- causer_name: nullable
- bank_code: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            total
            per_page
            current_page
            last_page
            next_page_url
            prev_page_url
            from
            to
            items: [
                {
                    created_at
                    bank_code
                    causer_name
                    item_name
                    valuitem_namee_before
                    value_after
                }
            ]
        }
    }

- 500:
    {
        status_code: 500,
        message: "Enigma bank setting logs cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ SettingLogController@index
- Service:
    + App\Services\SettingLogs\EnigmaBankSettingLogService
- Repository:
    + App\Repositories\Eloquent\SettingLogRepository
    
**7. Table get data**

- setting_logs
- setting_log_details

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Get csv of enigma bank setting logs
**1. URL**

- GET: {URL}/enigma-bank-setting-logs/csv

**2. Description**

- Get enigma bank setting log csv

**3. Last modify**

	// TO DO

**4. Parameter**

- per_page: nullable
- sort_column: nullable
- sort_direction: nullable
- created_at: nullable
- causer_name: nullable
- bank_code: nullable

> **5. Response**

```php
- 200:
    Returns the number of characters read from the handle and passed through to the output.( file csv with name have format enigma-bank_setting-log-date('YmdHis').csv)

- 500:
    {
        status_code: 500,
        message: "Enigma bank setting logs cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ SettingLogController@getEnigmaBankSettingLogCsv
- Service:
    + App\Services\SettingLogs\EnigmaBankSettingLogService
- Repository:
    + App\Repositories\Eloquent\SettingLogRepository
    
**7. Table get data**

- setting_logs
- setting_log_details

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Maintenance
### Get list enigma bank setting logs
**1. URL**

- GET: {URL}/maintenances

**2. Description**

- Show maintenance setting

**3. Last modify**

	// TO DO

**4. Parameter**

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            start_time
            end_time
            is_maintenance
        }
    }

- 500:
    {
        status_code: 500,
        message: "Maintenance setting cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ MaintenanceController@index
- Service:
    + App\Services\Maintenances\ShowService
- Repository:
    + App\Repositories\Eloquent\MaintenanceSettingRepository
    + App\Repositories\Eloquent\EnigmaSettingRepository
    
**7. Table get data**

- enigma_setting
- maintenance_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Update maintenance setting
**1. URL**

- POST: {URL}/maintenances

**2. Description**

- Update maintenance setting

**3. Last modify**

	// TO DO

**4. Parameter**

- start_time:
    + required
    + date_format:Y-m-d H:i
    + after:now
    + bail
    + regex:/30|00$/
- end_time:
    + required
    + date_format:Y-m-d H:i
    + after:start_time
    + bail
    + regex:/30|00$/
- description

> **5. Response**

```php
- 201:
    {
        status_code: 201,
        message: "",
        data: {
            id
        }
    }

- 422:
    {
        status_code: 422,
        message: "Data validation failed",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Update maintenance time error.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ MaintenanceController@store
- Service:
    + App\Services\Maintenances\UpdateService
- Repository:
    + App\Repositories\Eloquent\MaintenanceSettingRepository
    
**7. Table get data**

- maintenance_settings

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

## Bank Api logs
### Export CSV payment request
**1. URL**

- GET: {URL}/bank-api-logs/export-csv

**2. Description**

- Export CSV payment request

**3. Last modify**

	// TO DO

**4. Parameter**

- system_type: nullable
- company_code: nullable
- employee_code: nullable
- start_date: nullable
- end_date: nullable
- sort_direction: nullable
- employee_payment_request_id: nullable

> **5. Response**

```php
- 201:
    Returns the number of characters read from the handle and passed through to the output (file csv with name have format bank_api_logs-date('YmdHis').csv)
    
- 500:
    {
        status_code: 500,
        message: "Bank api logs cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BankApiLogController@exportCsv
- Service:
    + App\Services\BankApiLogs\ExportCsvService
    + App\Services\BankApiLogs\ShowService
- Repository:
    + App\Repositories\Eloquent\BankApiLogRepository
    
**7. Table get data**

- bank_api_logs

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### List bank api log
**1. URL**

- GET: {URL}/bank-api-logs

**2. Description**

- List bank api log

**3. Last modify**

	// TO DO

**4. Parameter**

- system_type: nullable
- company_code: nullable
- employee_code: nullable
- start_date: nullable
- end_date: nullable
- sort_direction: nullable
- employee_payment_request_id: nullable
- per_page: nullable
- last_evaluated_key: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            items: [
                {
                    body_request
                    body_response
                    created_at
                    message_error_detail
                    header_response
                    url
                    employee_code
                    updated_at
                    employee_payment_request_id
                    system_type: {
                        id
                        name
                    },
                    company_code
                    id
                    status
                    header_request
                }
            ]
        }
    }
- 500:
    {
        status_code: 500,
        message: "Bank api logs cannot be listed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BankApiLogController@index
- Service:
    + App\Services\BankApiLogs\ListService
    + App\Services\BankApiLogs\ShowService
- Repository:
    + App\Repositories\Eloquent\BankApiLogRepository
    
**7. Table get data**

- bank_api_logs

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Show bank api log
**1. URL**

- GET: {URL}/bank-api-logs/{id}

**2. Description**

- Show bank api log

**3. Last modify**

	// TO DO

**4. Parameter**

- created_at: nullable

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "",
        data: {
            body_request
            body_response
            created_at
            message_error_detail
            header_response
            url
            employee_code
            updated_at
            employee_payment_request_id
            system_type: {
                id
                name
            },
            company_code
            id
            status
            header_request
        }
    }
- 500:
    {
        status_code: 500,
        message: "Bank api logs cannot be showed.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BankApiLogController@show
- Service:
    + App\Services\BankApiLogs\ShowService
- Repository:
    + App\Repositories\Eloquent\BankApiLogRepository
    
**7. Table get data**

- bank_api_logs

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO

### Delete bank api log
**1. URL**

- DELETE: {URL}/bank-api-logs/{id}

**2. Description**

- Delete bank api log

**3. Last modify**

	// TO DO

**4. Parameter**

- created_at: nullable
- current_password: 
    + required
    + current_password

> **5. Response**

```php
- 200:
    {
        status_code: 200,
        message: "Bank api logs has been deleted successfully.",
        data: []
    }
    
- 403:
    {
        status_code: 403,
        message: "The current password does not match.",
        errors: []
    }
    
- 500:
    {
        status_code: 500,
        message: "Bank api logs cannot be deleted.",
        errors: []
    }
```

**6. Code handle**

- Controller - function:
	+ BankApiLogController@destroy
- Service:
    + App\Services\BankApiLogs\DeleteService
- Repository:
    + App\Repositories\Eloquent\BankApiLogRepository
    + App\Repositories\Eloquent\EnigmaUserRepository
    
**7. Table get data**

- bank_api_logs

**8. Table change data(insert, update, delete)**

	// TO DO

**9. SQL**
	
	// TO DO
