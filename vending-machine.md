---
title: Coding a vending machine
date: 2014-09-22
---

You wouldn't hire a chef without asking him to cook you something would you?  Surprisingly though, I'm very rarely asked to write code during interviews.

Personally, when I'm interviewing candidates for programming jobs, I quite like to ask the vending machine question.  We all use vending machines, they're rather simple: you put coins in, select a drink and the machine (hopefully) spits out your drink and some change.  Unless of course the drink gets stuck and you [shake the machine in rage](http://freakonomics.com/2011/09/08/how-are-sharks-less-dangerous-than-vending-machines-an-exercise-in-conditional-risk/) and thump your fist against the glass in desperation.  But we won't go there.

When you get thinking about the problem, you realize that there are a few subtleties.  What if the user selects a drink that doesn't exist?  What if the slot is empty?  How do you calculate the change?  What if there isn't enough change?

During an interview, there isn't really enough time to go into all the details that I've put down here, but the process is enough to throw light on a candidate's problem solving ability and methodology.

I like to hand candidates a white board pen and start with a bit of requirements analysis, in order to better understand the scope of the problem, so lets start there.

## Requirements

Our vending machine has a number of slots, which are numbered, each filled with various unhealthy fizzy drinks and industrial snacks.  Selected products are pushed forwards out of the slot and fall down to the trap, where the user retrieves his purchases.  The vending machine is also equipped with an LCD display, a keypad, a coin slot, a change button, and a change slot.

Now, the mechanics and electronics of the machine are out of scope, let's simply say that Jim Bob the electrical engineer has taken care of creating a micro-controller to handle that, we as programmers just need to make the machine think.

With that preamble out of the way, lets get down to our formal requirements:

* In the machine's initial state, the credit is zero, as displayed on the LCD.

* The user can insert coins, which increment the credit, as displayed on the LCD.  This is a European vending machine, so the coins accepted are: €2, €1, 50c, 20c, 10c, 5c.  The machine takes care of rejecting invalid coins.  The machine also rejects coins if the float is full.

* The user can hit the change button, which causes the machine to spit out the credit in change.  The credit is reset to zero.

* The user can enter a slot number using the keypad.

* If the slot is unrecognized, the word "Invalid" flashes on the LCD, before reverting to the credit.

* If the slot is empty, the word "Empty" flashes on the LCD, before reverting to the credit.

* If the credit is less than the price of the product in the slot, then the price flashes on the LCD for 1 second, before reverting to the credit.

* If the credit equals or exceeds the price of the product, then the product is served.  The credit is decremented by the price of the product.

* Change should be returned using the highest possible denomination coins.  So if the credit is 60c, then the returned coins should be 50c and 10c.  Unless no 50c coins are available, in which case three 20c coins should be returned, etc.

* The available slots and the prices of each slot are stored in a file.  This file is updated by an external process.

That's probably enough.  I find that simply writing out the requirements in this form forces one to think through all the possibilities.  Now that we know how the program should behave, we need to think about ensuring that the program does indeed behave this way.

## Tests

Here are a couple of test cases that we can perform to ensure compliance to our requirements (note that each test has a fresh start): 

* Correct initialization
	* Ensure that the credit is zero after startup.  
* Invalid product
	* Attempt to purchase an inexistant product.
	* Ensure that the message Invalid is displayed.  
* Empty slot 
	* Simulate an empty slot.
	* Attempt to purchase this product.
	* Ensure that the message Empty is displayed.
* Insufficient credit
	* Insert €1.  
	* Attempt to purchase a product for $1.50.
	* Ensure that the price of the product is displayed.
	* Ensure that the credit remains €1.
* Dispense coins normally
	* Insert coins, say 50c, 20c and 10c.  
	* Ensure that credit is 80c.
	* Dispense the change, ensuring that the coins are dispensed in the same denominations.
	* Ensure that the credit is now zero again.
* Normal purchase
	* Insert €1.  
	* Ensure that credit is €1.
	* Purchase a product for 80c.
	* Dispense the change, ensuring that a 20c coin is returned.
	* Ensure that the credit is now zero again.
* Normal purchase, no 20c coins available
	* Insert €1.  
	* Purchase a product for 80c.
	* Simulate the absence of 20c coins.
	* Dispense the change, ensuring that two 10c coins are returned.
	* Ensure that the credit is now zero again.

That should about cover it.  The tricky thing when writing tests is to avoid repeating yourself while still covering all the possibilities.  Note that I've tested both for success and for failure.

It becomes apparent that we need to be able to simulate the absence of products as well as the absence of certain coins, and also identify the coins that are actually dispensed.

## Pseudocode

I'm not religious about pseudocode, but it's a tool that I often use when writing complicated programs.  Generally I write the pseudocode in a block comment and then transform it into code.

Here's my first attempt:

	Credit = 0
	Status = ""

	SlotPrices = [0.8, 0.9, 1.2, 1, 1.5]
	// to be read from file at startup

	CoinDenominations = [2, 1, 0.5, 0.2, 0.1, 0.05]

	InsertCoin ( value )
	// called by the machine when coins are accepted
	// increment Credit by value

	SelectSlot ( slot )
	// called when the user enters a selection on the keypad
	// check if slot is valid
	   // if not, flash "Invalid" and exit
	// get the price of the slot
	// check if the user has sufficient credit
	   // if not, flash the price and exit
	// try to dispense the product
	   // if failed, flash "Empty" and exit
	// decrement the credit by price

	IsSlotValid ( slot )
	// check if we have a price for the requested slot

	GetSlotPrice ( slot )
	// return the price of the requested slot

	HasSufficientCredit ( price )
	// return true if credit equals or exceeds price, otherwise false

	TryDispenseProduct ( slot )
	// send message to machine to dispense product
	// returns true if product dispensed, false if product not avaiable

	DispenseChange ( )
	// called when the user hits the change button
	// starting at the highest coin denomination
	// check if the coin value exceeds the credit
		// if so, skip to next highest coin
	   		// if no coins left, exit
	// try to dispense the coin
	   // if failed, skip to next highest coin
	   		// if no coins left, exit
	// decrement credit by coin value
	// check if credit remains
	   // if not, exit

	TryDispenseCoin ( value )
	// send message to machine to dispense coin
	// returns true if coin dispensed, false if coin not available

	IncrementCredit ( value )

	DecrementCredit ( value )
	
	FlashStatus ( message )
	// display the message for a short interval

You'll notice a few things:

* I've hard-coded the coin denominations, which is probably alright, as we've agreed that this is a European vending machine.  We can always customize this later.  

* I've given my functions readily-understandable names, generally in Verb-Noun or Noun-Verb form.

* I've been liberal with my comments.

* There is a caveat in the DispenseChange function.  In our requirements we stated that "The credit is reset to zero" after the change is dispensed, but what if we need to give 10c change, and there are no 10c or 5c coins available?  Per our current solution, the credit will remain at 10c.  This could be confusing; we probably need to flash the words "No Coin", so that user will give up and leave instead of bashing the coin return button all day.

It's time to update the requirements!  Let's review that third line:

* The user can hit the change button, which causes the machine to spit out the credit in change.  The credit is reset to zero, *or, if insufficient coins are available, the words "No Coin" flash on the LCD.*

And I'll update my pseudocode, adding the following comment to the end of DispenseChange():

	// After dispensing change, if credit exceeds zero, flash "No Coin"

I also need to add an additional test case:

* Normal purchase, no 20c, 10c and 5c coins available
	* Insert €1.  
	* Purchase a product for 80c.
	* Simulate the absence of 20c, 10c and 5c coins.
	* Attempt to dispense the change, ensuring that no coins are returned.
	* Ensure that the message No Coin is displayed.
	* Ensure that the credit is 20c.

I think the key here is to recognize that requirements always change.  Discovering pitfalls earlier saves time later.

## Code

Having written our requirements and the accompanying pseudocode, as well as our tests, it's time now to write some actual code.  I'm going to use Javascript, because it ubiquitous these days.  This is a website, so I'm going to use Angular as well.  That said, were I programming a real vending machine, I'd use something different.

Let's start with our controller logic:

	'use strict';

	angular.module('myApp', [])

	.controller('vendingMachineController', ['$scope', '$interval', '$filter', 'vendingMachineIO', function($scope, $interval, $filter, vendingMachineIO) {

		$scope.credit = 0;
		$scope.status = "";
		$scope.slotPrices = vendingMachineIO.getSlotPrices();
	
		$scope.coinDenominations = [2, 1, 0.5, 0.2, 0.1, 0.05];

		// called by the machine when coins are accepted
		$scope.insertCoin = function(value) {
			incrementCredit(value);
		}

		// called when the user enters a selection on the keypad
		$scope.selectSlot = function(slot) {
			if (!isSlotValid(slot)) return;
			var price = getSlotPrice(slot);
			if (!hasSufficientCredit(price)) return;
			if (!dispenseProduct(slot)) return;
			decrementCredit(price);
			flashStatus("Enjoy!");
		}

		function isSlotValid(slot) {
			if (slot < $scope.slotPrices.length) return true;
			flashStatus("Invalid"); return false;
		}

		function getSlotPrice(slot) {
			return $scope.slotPrices[slot];
		}

		function hasSufficientCredit(price) {
			if ($scope.credit >= price) return true;
			flashPrice(price); return false; 
		}
	
		function dispenseProduct(slot) {
			if (vendingMachineIO.tryDispenseProduct(slot)) return true;
			flashStatus("Empty"); return false; 
		}
	
		// called when the user hits the change button
		$scope.dispenseChange = function() {
			$scope.coinDenominations.forEach(function(value) {
				while (dispenseCoin(value)) { };
			});
			if ($scope.credit > 0) flashStatus("No Coin"); // Unable to return all change
		}
	
		function dispenseCoin(value) {
			if (value > $scope.credit) return false; // Coin too big
			if (!vendingMachineIO.tryDispenseCoin(value)) return false; // No coins left
			decrementCredit(value); return true; // OK - coin dispensed
		}

		function incrementCredit(value) {
			$scope.credit += value;
			roundTwoDecimals();
		}
	
		function decrementCredit(value) {
			$scope.credit -= value;
			roundTwoDecimals();
		}
	
		function roundTwoDecimals() {
			// Round to two decimal places to avoid floating point weirdness.
			$scope.credit = Math.round($scope.credit * 100) / 100
		}
	
		function flashPrice(price) {
			flashStatus($filter('currency')(price, "€")); 
		}
	
		function flashStatus(message) {
			$scope.status = message;
			$interval(function() { $scope.status = ""; }, 1000, 1);
		}
	
	}])

	.factory('vendingMachineIO', function() {	
		return {
			getSlotPrices: function() {
				// todo : read slot prices from file
				return [0.8, 0.9, 1.2, 1, 1.5];
			},
			tryDispenseProduct: function(slot) {
				// todo : send message to machine to dispense product
				return (slot !== 1);
			},
			tryDispenseCoin: function(value) {
				// todo : send message to machine to dispense coin
				return (value !== 0.05);
			}
		};
	});

A few remarks on the code:

* Function names are meaningful and function definitions are as short as possible, eliminating the need for most comments.
* To furthest extent possible, the code reads from top to bottom like a story.
* Communication with the underlying machine (Jim Bob's micro-controller) has been factored out into a separate service (vendingMachineIO).  These functions are just stubs that return dummy values used by our end-to-end tests later on.  Normally, some funky IO code with bizarre bit masks and binary addresses would go here.

## Unit tests

Now follows some unit tests, written in Jasmine, following the test cases that I defined earlier.

	'use strict';

	describe('myApp', function() {

		beforeEach(module('myApp'));
	
		describe('vendingMachineController', function() {

			var scope;
			var coins;
			var products;

			beforeEach(inject(function($rootScope, $controller, $interval, $filter, vendingMachineIO) {
				scope = $rootScope.$new();
				$controller('vendingMachineController', {
					$scope: scope, 
					$interval: $interval,
					$filter: $filter,
					vendingMachineIO: simulateVendingMachineIO(vendingMachineIO)
				});
			}));
		
			function simulateVendingMachineIO(io) {
				simulateSlotPrices(io);
				simulateCoinDispensing(io);
				simulateProductDispensing(io);
				return io;
			}
		
			function simulateSlotPrices(io) {
				spyOn(io, "getSlotPrices").andReturn([0.8, 0.9, 1.2, 1, 1.5]);
			}
		
			function simulateCoinDispensing(io) {
				coins = {
					canDispense: function(value) { return true; },
					dispensed: []
				};
				spyOn(io, "tryDispenseCoin").andCallFake(function(value) {
					if (!coins.canDispense(value)) return false;
					coins.dispensed.push(value); return true;
				});
			}
		
			function simulateProductDispensing(io) {
				products = {
					canDispense: function(slot) { return true; },
					dispensed: []
				};
				spyOn(io, "tryDispenseProduct").andCallFake(function(slot) {
					if (!products.canDispense(slot)) return false;
					products.dispensed.push(slot); return true;
				});
			}
			
			it('should initialize correctly', function() {		
				expect(scope.credit).toBe(0);
			});
		
			it('should not allow the purchase of an invalid product', function() {
				scope.selectSlot(5);
				expect(scope.status).toBe("Invalid");
			});

			it('should not allow a purchase from an empty slot', function() {
				scope.insertCoin(1);
				products.canDispense = function(slot) { 
					return (slot !== 1); // Disallow dispensing products from slot #1
				};
				scope.selectSlot(1);
				expect(scope.status).toBe("Empty");
			});
		
			it('should not allow a purchase with insufficient credit', function() {
				scope.insertCoin(1);
				scope.selectSlot(4);
				expect(scope.status).toBe("€1.50");
			});
		
			it('should dispense coins normally', function() {
				coins.inserted = [0.5, 0.2, 0.1];
				coins.inserted.forEach(scope.insertCoin);
				expect(scope.credit).toBe(0.8);
				scope.dispenseChange();
				expect(coins.dispensed.toString()).toBe(coins.inserted.toString());
				expect(scope.credit).toBe(0);
			});
		
			it('should allow a normal purchase', function() {
				insertCoinSelectFirstSlotAndDispenseChange();
				expect(coins.dispensed.toString()).toBe([0.2].toString());
				expect(scope.credit).toBe(0);
			});

			it('should allow a normal purchase, no 20c coins available', function() {
				coins.canDispense = function(value) { 
					return value !== 0.2; // Disallow dispensing 20c coins
				};
				insertCoinSelectFirstSlotAndDispenseChange();
				expect(coins.dispensed.toString()).toBe([0.1, 0.1].toString());
				expect(scope.credit).toBe(0);
			});
		
			it('should allow a normal purchase, no 20c, 10c and 5c coins available', function() {
				coins.canDispense = function(value) { 
					return value > 0.2; // Disallow dispensing 20c coins and smaller
				};
				insertCoinSelectFirstSlotAndDispenseChange();
				expect(coins.dispensed.length).toBe(0);
				expect(scope.credit).toBe(0.2);
				expect(scope.status).toBe("No Coin");
			});
		
			function insertCoinSelectFirstSlotAndDispenseChange() {
				scope.insertCoin(1);
				expect(scope.credit).toBe(1);
				scope.selectSlot(0);
				expect(scope.credit).toBe(0.2);
				scope.dispenseChange();
			}
				
		});

	});

A few remarks on the unit tests:

* A simulator has been created for the vending machine IO.  Function calls are redirected using Jasmine spies.  By default, products and coins are always dispensed, but each unit test is free to override this behaviour.
* Each unit test starts with a fresh copy of the controller object.
* While unit tests are not production code, they still need to be maintained.  Therefore, duplication is factored out into shared functions; such is the case of the insertCoinSelectFirstSlotAndDispenseChange() function.

## User interface

For completeness, I'm going to create a simple UI in HTML.  I'm not a designer, so you'll have to forgive the austerity of my interface.

	<div ng-app="myApp" ng-controller="vendingMachineController" class="my-app" style="margin-left:20px">

	<style>
	input[type=button] {
		margin-right:10px;
		width:50px;
	}
	</style>

	<h1>Unhealthy drinks &amp; snacks!</h1>

	<p>
	Please insert coin :
	</p>
	<p>
	<input type="button" value="€2" ng-click="insertCoin(2)"></input>
	<input type="button" value="€1" ng-click="insertCoin(1)"></input>
	<input type="button" value="50c" ng-click="insertCoin(0.5)"></input>
	</p>
	<p>
	<input type="button" value="20c" ng-click="insertCoin(0.2)"></input>
	<input type="button" value="10c" ng-click="insertCoin(0.1)"></input>
	<input type="button" value="5c" ng-click="insertCoin(0.05)"></input>
	</p>

	<p>
	<input id="credit" type="text" readonly value="{{credit | currency:'&euro;'}}"></input>
	<input type="button" ng-click="dispenseChange()" value="Change" style="width:100px"></input>
	</p>

	<p>
	<input id="status" type="text" readonly value="{{status}}"></input>
	</p>

	1: Coke - 80c<br/>
	2: Pepsi - 90c<br/>
	3: Candy bar - €1.20<br/>
	4: Lollipops - €1<br/>
	5: Jelly beans - €1.50<br/>

	<p>
	<input type="button" value="1" ng-click="selectSlot(0)"></input>
	<input type="button" value="2" ng-click="selectSlot(1)"></input>
	<input type="button" value="3" ng-click="selectSlot(2)"></input>
	</p>
	<p>
	<input type="button" value="4" ng-click="selectSlot(3)"></input>
	<input type="button" value="5" ng-click="selectSlot(4)"></input>
	<input type="button" value="6" ng-click="selectSlot(5)"></input>
	</p>
	<p>
	<input type="button" value="7" ng-click="selectSlot(6)"></input>
	<input type="button" value="8" ng-click="selectSlot(7)"></input>
	<input type="button" value="9" ng-click="selectSlot(8)"></input>
	</p>
	<p>
	<input type="button" value="0" style="visibility: hidden;"></input>
	<input type="button" value="0" ng-click="selectSlot(9)"></input>
	</p>

	</div>

You can see a live example on JsFiddle here: [http://jsfiddle.net/sm8cgsfh/1/](http://jsfiddle.net/sm8cgsfh/1/)

## End-to-end tests

The UI needs to be tested as well, so I've written some end-to-end tests using Protractor.

	'use strict';

	describe('myApp', function() {

		describe('vendingMachineView', function() {

			var elements;
	
			beforeEach(function() {
				browser.get('index.html');
				elements = {
					credit: element(by.id('credit')),
					status: element(by.id('status')),
					insert100: element(by.buttonText('€1')),
					insert050: element(by.buttonText('50c')),
					insert020: element(by.buttonText('20c')),
					insert010: element(by.buttonText('10c')),
					insert005: element(by.buttonText('5c')),
					slot1: element(by.buttonText('1')),
					slot2: element(by.buttonText('2')),
					slot5: element(by.buttonText('5')),
					slot6: element(by.buttonText('6')),
					change: element(by.buttonText('Change'))
				};
			});
		
			function expectCredit(expected) {
				expect(elements.credit.getAttribute('value')).toEqual(expected);
			}

			function expectStatus(expected) {
				expect(elements.status.getAttribute('value')).toEqual(expected);
			}

			it('should initialize correctly', function() {
				expectCredit('€0.00');
			});
		
			it('should not allow the purchase of an invalid product', function() {
				elements.slot6.click();
				expectStatus('Invalid');
			});
		
			it('should not allow a purchase from an empty slot', function() {
				elements.insert100.click();
				elements.slot2.click();
				expectStatus('Empty');
			});
		
			it('should not allow a purchase with insufficient credit', function() {
				elements.insert100.click();
				elements.slot5.click();
				expectStatus('€1.50');
			});
		
			it('should dispense coins normally', function() {		
				elements.insert050.click();
				elements.insert020.click();
				elements.insert010.click();
				expectCredit('€0.80');
				elements.change.click();
				expectCredit('€0.00');
			});

			it('should allow a normal purchase', function() {
				elements.insert100.click();
				elements.slot1.click();
				expectCredit('€0.20');
				elements.change.click();
				expectCredit('€0.00');
				expectStatus('Enjoy!');
			});
		
			it('should not allow 5c coins to be returned', function() {
				elements.insert005.click();
				expectCredit('€0.05');
				elements.change.click();
				expectCredit('€0.05');
				expectStatus('No Coin');
			});

		});

	});

A few remarks on the end-to-end tests:

* The tests resemble the unit tests written earlier, except this time we're interacting with the actual UI elements which are in turn interacting with the controller.
* The UI doesn't show the change being dispensed, so it's not possible to verify that the correct change is returned.

## Wrapping up

I've broken down the process of solving the given problem into a number of steps: first we analyze the requirements, then we think about testing, then we write some code and unit tests, then we create the user interface and end-to-end tests.  This process isn't always sequential: unexpected problems may arise while developing which may in turn force the requirements to change.

In my career I've seen many a programmer who jumps straight into the code, without fully understanding the requirements and without thinking about how to test the result.  Generally this leads to code which is untestable, and probably unmaintainable too.

Copy and paste are the worst tools for programming: eliminate duplication by placing common code in shared functions.  Make code self-documenting by using meaningful function and variable names.  Keep functions short, 7 lines is probably a good maximum.  Try to arrange functions from top to bottom to facilitate reading.  Tests are code too, and the same rules apply.

The full source code from this article is [available on Github](https://github.com/patrickjohncollins/vending-machine).