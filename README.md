# Description

Angular2 CC Library - for validation and formating of input parameters

# Demo
1. Clone repo
2. run `npm install`
3. run `npm run dev`
4. visit `http://localhost:4200`

# Usage

## Formating Directive
On the input fields, add the specific directive to format inputs. 
All fields must be `type='tel'` in order to support spacing and additional characters

**Credit Card Formater**
* add `ccNumber` directive:
```
<input id="cc-number" type="tel" autocomplete="cc-number" ccNumber>
```

**Expiration Date Formater**
Will support format of MM/YY or MM/YYYY
* add `ccExp` directive:
```
<input id="cc-exp-date" type="tel" autocomplete="cc-exp" ccExp>
```

**CVC Formater**
* add `ccCvc` directive:
```
<input id="cc-cvc" type="tel" autocomplete="off" ccCvc>
```

### Validation
Current only Model Validation is supported.
To implement, import the validator library and apply the specific validator on each form control
```
import { CreditCardValidator } from '../../src/validators/credit-card.validator';

@Component({
  selector: 'app',
  template: `
    <form #demoForm="ngForm" (ngSubmit)="onSubmit(demoForm)" novalidate>
        <input id="cc-number" formControlName="creditCard" type="tel" autocomplete="cc-number" ccNumber>
        <input id="cc-exp-date" formControlName="expirationDate" type="tel" autocomplete="cc-exp" ccExp>
        <input id="cc-cvc" formControlName="cvc" type="tel" autocomplete="off" ccCvc>
    </form>
  `
})
export class AppComponent implements OnInit {
  form: FormGroup;
  submitted: boolean = false;

  constructor(private _fb: FormBuilder) {}

  ngOnInit() {
    this.form = this._fb.group({
      creditCard: ['', [<any>CreditCardValidator.validateCCNumber]],
      expirationDate: ['', [<any>CreditCardValidator.validateExpDate]],
      cvc: ['', [<any>Validators.required, <any>Validators.minLength(3), <any>Validators.maxLength(4)]] 
    });
  }

  onSubmit(form) {
    this.submitted = true;
    console.log(form);
  }
}
```

# Inspiration

Based on Stripe's [jquery.payment](https://github.com/stripe/jquery.payment) plugin but adapted for use by Angular2

# License

MIT

# TODO
- [ ] Documentation
- [x] Unit Tests
- [ ] Protractor Tests
- [ ] Validate cvc input length against value from card object
- [ ] publish to NPM
