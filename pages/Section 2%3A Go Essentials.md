# Values and Types
	- ## Basic Types
		- Go comes with a couple of built-in basic types:
			- `int` - a number *WITHOUT* decimal places (eg. -5, 10, 12, etc.)
			- `float64` - a number *WITH* decimal places (eg. -4.2, 10.012, 12.0, etc.)
			- `string` - a text value c1reated via double quotes or backticks (eg.
			  "Hello World" or \`Hello World\`)
			- `bool` - `true` or `false`
	- ## Limitations of `fmt.Scan()`
		- The `fmt.Scan()` function is a great function for easily fetching & using user
		  input through the command line.
		- But this function also has an **important limitation**: You can't (easily)
		  fetch **multi-word input values*. Fetching text that consists of more than a
		  single word is tricky with this function.
- ## Conditional and `for` Loops
	- The `for` loop in Go comes in a few variations. Some basic ones are:
		- ```go
		  for i := 0; i < n; i++ {
		  
		  }
		  ```
	- or as an infinite loop, which replaces `while` in Go:
		- ```go
		  for {
		  
		  }
		  ```
	- and finally as:
		- ```go
		  for someCondition {
		     
		  }
		  ```
	- `someCondition` is an expression that yields a boolean value or variable that
	    contains a boolean value. In that case, the loop will continue to execute the
	    code nested in the loop until the codition/variable yields `false`
- # Examples
	- ## Investment Calculator
		- ```go
		  // investment_calculator.go
		  package main
		  
		  import (
		      "fmt"
		      "math"
		  )
		  
		  const inflationRate float64 = 2.5
		  
		  func main() {
		      //! ====Go can automatically infer the type.====
		      // investmentAmount := 1000
		  
		      //! ====It is also possible to declare a type.====
		      // var investmentAmount float64 = 1000
		  
		      //! ====Instatiate the variable and can edit it later====
		      var investmentAmount float64
		  
		      //! ====It is possible to declare multiple variables in one go====
		      // var investmentAmount, years float64 = 1000, 10
		      // investmentAmount, years := 1000.0, 10.0
		      // investmentAmount, years, expectedReturnRate := 1000.0, 10.0, 5.5
		  
		      //! ====In some cases it is also possible to define variables of different types====
		      // var investmentAmount, years = 1000, "10"
		  
		      var expectedReturnRate float64 = 5.5
		  
		      // float64 := 10
		      var years float64 = 10
		  
		      //! ====Constants can be declared with const keyword====
		      // const inflationRate float64 = 2.5
		  
		      fmt.Print("Investment Amount: ")
		      //! ====Need to pass pointers for fmt.Scan====
		      fmt.Scan(&investmentAmount)
		  
		      // fmt.Print("Expected Return Rate: ")
		      outputText("Expected Return Rate: ")
		      fmt.Scan(&expectedReturnRate)
		  
		      fmt.Print("Investment Horizon: ")
		      fmt.Scan(&years)
		  
		      // ====Can convert variables between data types with a wrap float64() or int()====
		      // futureValue := float64(investmentAmount) * math.Pow(1+expectedReturnRate/100, float64(years))
		      // var futureValue float64 = investmentAmount * math.Pow(1+expectedReturnRate/100, years)
		      // var futureRealValue float64 = futureValue / math.Pow(1+inflationRate/100, years)
		  
		      //! ====Use function to calculate future values instead====
		      futureValue, futureRealValue := calculateFutureValues(investmentAmount, expectedReturnRate, years)
		  
		      // ====Different ways to print statements====
		      // fmt.Println("Future value before inflation:", futureValue)
		      // fmt.Println("Future value after inflation:", futureRealValue)
		      // fmt.Printf("Future Value: %v\nFuture Value after Inflation: %v\n", futureValue, futureRealValue)
		      // fmt.Printf("Future Value: %.0f\nFuture Value after Inflation: %.1f\n", futureValue, futureRealValue) //0.xf (where x is d.p.)
		  
		      formattedFV := fmt.Sprintf("Future Value: %.0f\n", futureValue)
		      formattedRFV := fmt.Sprintf("Future Value after Inflation: %.1f\n",
		                                  futureRealValue)
		      fmt.Print(formattedFV, formattedRFV)
		      // fmt.Printf(`Future Value: %.0f
		      // Future Value after Inflation: %.1f`, futureValue, futureRealValue)
		  }
		  
		  func outputText(text string) {
		      fmt.Print(text)
		  }
		  
		  // func calculateFutureValues(investmentAmount float64, expectedReturnRate float64, years float64) (float64, float64) {
		  // 	fv := investmentAmount * math.Pow(1+expectedReturnRate/100, years)
		  // 	rfv := fv / math.Pow(1+inflationRate/100, years)
		  // 	return fv, rfv
		  // }
		  
		  // ====Another way to define functions with outputs====
		  func calculateFutureValues(investmentAmount float64, expectedReturnRate float64, years float64) (fv float64, rfv float64) {
		      fv = investmentAmount * math.Pow(1+expectedReturnRate/100, years)
		      rfv = fv / math.Pow(1+inflationRate/100, years)
		      return
		  }
		  ```
	- ## Profit Calculator
		- ```go
		  package main
		  
		  import (
		  	"errors"
		  	"fmt"
		  	"os"
		  )
		  
		  func main() {
		  	// var revenue, expenses, taxRate float64
		  
		  	// fmt.Print("Revenue: ")
		  	// fmt.Scan(&revenue)
		  	//
		  	// fmt.Print("Expenses: ")
		  	// fmt.Scan(&expenses)
		  	//
		  	// fmt.Print("Tax Rate: ")
		  	// fmt.Scan(&taxRate)
		  
		  	revenue, err := getUserInput("Revenue: ")
		  	if err != nil {
		  		fmt.Println("Error:", err)
		  		panic("Revenue is not valid.")
		  	}
		  
		  	expenses, err := getUserInput("Expenses: ")
		  	if err != nil {
		  		fmt.Println("error:", err)
		  		panic("expenses is not valid.")
		  	}
		  
		  	taxRate, err := getUserInput("Tax Rate: ")
		  	if err != nil {
		  		fmt.Println("Error:", err)
		  		panic("Tax Rate is not valid.")
		  	}
		  
		  	// var earnBeforeTax float64 = revenue - expenses
		  	// var earnAfterTax float64 = earnBeforeTax * (1 - taxRate/100)
		  	// var ratio float64 = earnBeforeTax / earnAfterTax
		  	earnBeforeTax, earnAfterTax, ratio := calculateFinancials(revenue, expenses, taxRate)
		  
		  	// fmt.Println("EBT:", earnBeforeTax)
		  	// fmt.Println("Profit:", earnAfterTax)
		  	// fmt.Println("Ratio:", ratio)
		  
		  	fmt.Printf("EBT: %.1f\nProfit: %.1f\nRatio: %.1f\n", earnBeforeTax, earnAfterTax, ratio)
		  	writeResultsToFile(earnBeforeTax, earnAfterTax, ratio)
		  }
		  
		  func calculateFinancials(revenue, expenses, taxRate float64) (float64, float64, float64) {
		  	var earnBeforeTax float64 = revenue - expenses
		  	var earnAfterTax float64 = earnBeforeTax * (1 - taxRate/100)
		  	var ratio float64 = earnBeforeTax / earnAfterTax
		  	return earnBeforeTax, earnAfterTax, ratio
		  }
		  
		  func getUserInput(infoText string) (float64, error) {
		  	var userInput float64
		  	fmt.Print(infoText)
		  	fmt.Scan(&userInput)
		  
		  	if userInput <= 0 {
		  		return 0, errors.New("Value cannot be less than or equal to zero (0).")
		  	}
		  
		  	return userInput, nil
		  }
		  
		  func writeResultsToFile(earnBeforeTax, earnAfterTax, ratio float64) {
		  	var output string = fmt.Sprintf("EBT: %.2f\nProfit: %.2f\nRatio: %.2f\n",
		                                      earnBeforeTax,
		                                      earnAfterTax,
		                                      ratio)
		  	os.WriteFile("results.txt", []byte(output), 0644)
		  }
		  ```
	- ## Bank
		- ```go
		  package main
		  
		  import (
		  	"errors"
		  	"fmt"
		  	"os"
		  	"strconv"
		  )
		  
		  const accountBalanceFile string = "balance.txt"
		  
		  func writeBalanceToFile(balance float64) {
		  	var balanceText string = fmt.Sprint(balance)
		  	os.WriteFile(accountBalanceFile, []byte(balanceText), 0644) // 0644 = read and write
		  }
		  
		  func getBalanceFromFile() (float64, error) {
		  	data, err := os.ReadFile(accountBalanceFile)
		  	if err != nil {
		  		return 1000, errors.New("Failed to find/read file.")
		  	}
		  
		  	var balanceText string = string(data)
		  	balance, err := strconv.ParseFloat(balanceText, 64)
		  	if err != nil {
		  		return 1000, errors.New("Failed to parse balance.")
		  	}
		  
		  	return balance, nil
		  }
		  
		  func main() {
		  	accountBalance, err := getBalanceFromFile()
		  	if err != nil {
		  		fmt.Println("Error:", err)
		          // panic("Cannot continue, sorry.")
		          fmt.Println("Continuing with default balance of $1000.00")
		  	}
		  
		  	fmt.Println("Welcome to Go Bank!")
		  
		  	// for i := 0; i < 2; i++ {
		  	for {
		  		fmt.Println("=====================")
		  
		  		fmt.Println("What do you want to do?")
		  		fmt.Println("1. Check Balance")
		  		fmt.Println("2. Deposit Money")
		  		fmt.Println("3. Withdraw Money")
		  		fmt.Println("4. Exit")
		  
		  		var choice int
		  		fmt.Print("Select Option: ")
		  		fmt.Scan(&choice)
		  		fmt.Println("You Chose: ", choice)
		  
		  		if choice == 1 {
		  			fmt.Printf("Your balance is $%.2f\n", accountBalance)
		  		} else if choice == 2 {
		  			fmt.Print("Enter amount to deposit: ")
		  			var depositAmount float64
		  			fmt.Scan(&depositAmount)
		  
		  			if depositAmount <= 0 {
		  				fmt.Println("Invalid amount. Amount must be greater than 0.")
		  				continue
		  			}
		  
		  			accountBalance += depositAmount
		  			fmt.Printf("Your new balance is $%.2f\n", accountBalance)
		  			writeBalanceToFile(accountBalance)
		  		} else if choice == 3 {
		  			fmt.Print("Enter amount to withdraw: ")
		  			var withdrawAmount float64
		  			fmt.Scan(&withdrawAmount)
		  
		  			if withdrawAmount <= 0 {
		  				fmt.Println("Invalid amount. Amount must be greater than 0.")
		  				continue
		  			}
		  			if withdrawAmount > accountBalance {
		  				fmt.Println("Insufficient funds.")
		  				continue
		  			}
		  
		  			accountBalance -= withdrawAmount
		  			fmt.Printf("Your new balance is $%.2f\n", accountBalance)
		  			writeBalanceToFile(accountBalance)
		  		} else {
		  			fmt.Println("Exiting...")
		  			break
		  		}
		  
		  		//! ====In Go, the break statement cannot exit the loop from inside a switch statement====
		  		//! ====this is because the break statement simply breaks the switch statement====
		  		// switch choice {
		  		// case 1:
		  		// 	fmt.Printf("Your balance is $%.2f\n", accountBalance)
		  		// case 2:
		  		// 	fmt.Print("Enter amount to deposit: ")
		  		// 	var depositAmount float64
		  		// 	fmt.Scan(&depositAmount)
		  		//
		  		// 	if depositAmount <= 0 {
		  		// 		fmt.Println("Invalid amount. Amount must be greater than 0.")
		  		// 		continue
		  		// 	}
		  		//
		  		// 	accountBalance += depositAmount
		  		// 	fmt.Printf("Your new balance is $%.2f\n", accountBalance)
		  		// case 3:
		  		// 	fmt.Print("Enter amount to withdraw: ")
		  		// 	var withdrawAmount float64
		  		// 	fmt.Scan(&withdrawAmount)
		  		//
		  		// 	if withdrawAmount <= 0 {
		  		// 		fmt.Println("Invalid amount. Amount must be greater than 0.")
		  		// 		continue
		  		// 	}
		  		// 	if withdrawAmount > accountBalance {
		  		// 		fmt.Println("Insufficient funds.")
		  		// 		continue
		  		// 	}
		  		//
		  		// 	accountBalance -= withdrawAmount
		  		// 	fmt.Printf("Your new balance is $%.2f\n", accountBalance)
		  		// default:
		  		// 	fmt.Println("Exiting...")
		  		// 	return
		  		// }
		  	}
		  }
		  ```