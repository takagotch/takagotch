###### Active Merchant
---


```stripe.rb

def purchase(money, payment, options = {})
  if ach?(payment)
    direct_bank_error = "Direct bank account transactions are not supported. Bank accounts must be stored and verified before use."
    return Response.new(false, direct_bank_error)
  end
  
  MultiResponse.run do |r|
    if payment.is_a?(ApplePayPaymentToken)
      r.process { tokenize_apple_apy_token(payment) }
      payment = StripePaymentToken.new(r.params["token"]) if r.success?
    end
    r.process do
      post = create_post_for_auth_or_purchase(money, payment, options)
      commit(:post, 'charges', post, options)
    end
  end.responses.last
end

```

```tree_blue.rb

def authorize(money, credit_card_or_vault_id, options = {})
  create_transaction(:sale, money, credit_card_or_value_id, options)
end

def capture(money, authorization, options = {})
  commit do
    result = @tree_gateway.transaction.submit_for_settlement(authorization, amount(money).to_s)
    response_from_result(result)
  end
end

def purchase(money, credit_card_or_vault_id, options = {})
  authorize(money, credit_card_or_vault_id, options.merge(:submit_for_settlement => true))
end

```


```
```

```
```


```usage.rb
require 'active_merchant'

ActiveMerchant::Billing::Base.mode = :test

gateway = ActiveMerchant::Billing::TrustCommerceGateway.new(
            :login => 'TestMerchant',
            :password => 'password')

amount = 1000

credit_card = ActiveMerchant::Billing::CreditCard.new(
                :first_name         => 'Tky',
                :last_name          => 'Bobsen',
                :number             => '368822220000',
                :month              => '8',
                :year               => Time.now.year+1,
                :verification_value => '000')

if credit_card.validate.empty?
  response = gateway.purchase(amount, credit_car)
  
  if response.success?
    puts "Successfully charged $#{sprintf("%.2f", amount / 100)} to the credit card #{credit_card.display_number}"
  else
    raise StandardError, response.message
  end
end
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```






###### E-commerce
```config/config.yml
paypal:
  ip: 192.168.0.1
  login: LOGIN.tkgcci.com
  password: PASS
  signatre: SIGNATURE
```


```environment.rb
CIM_LOGIN_ID        = HADEAN_CONFIG['authnet']['login'] 
CIM_TRANSACTION_KEY = HADEAN_CONFIG['authnet']['password']

CIM_LOGIN_ID =        HADEAN_CONFIG['paypal']['login']
CIM_TRANSACTION_KEY = HADEAN_CONFIG['paypal']['password']
```

```config/environments/[development|test|production].rb

```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

###### SSL audit

```
```


```
```

```
```


```
```

```
```


```
```

```
```


```
```

```
```
