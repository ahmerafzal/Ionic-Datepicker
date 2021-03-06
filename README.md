
<h1>Ionic / AngularJS Datepicker</h1>
In this Datepicker i am using the ionic popup to show datepicker fields and buttons for selecting date
<br /><br />
![Alt text](https://raw.githubusercontent.com/ahmerafzal/Ionic-Datepicker/master/picker.png "Ionic Datepicker")


<h2>Implement this input in your template file</h2>
```html
<input type="text" ng-click="DatePicker($event)" readonly />
```

<h2>Place this script in your controller</h2>
```javascript
$scope.DatePicker = function(e) {
		$model = $(e.target).attr('ng-model');
		$value = $(e.target).val();
		$scope.data = {}
		function daysInMonth(month,year) {
			return new Date(year, month, 0).getDate();
		}
		var d = new Date();
		var tomorrow = new Date();
		var tomorrowDate = (d.getDate()+1);
		var tomorrowMonth = (d.getMonth()+1);
		var tomorrowYear = d.getFullYear();
		var totalDaysInMonth = daysInMonth(tomorrowMonth,tomorrowYear);
		if(d.getDate() == totalDaysInMonth) {
			var tomorrowDate = 1;		
			if(tomorrowMonth == 12) {
				tomorrowYear = (d.getFullYear()+1);
			}
		}	
		if($value=='') {
			$scope.data.pickup_date = tomorrowDate;
			$scope.data.pickup_month = tomorrowMonth;
			$scope.data.pickup_year = tomorrowYear;
		} else {
			$prevSelectedDate = $value.split("/");
			$scope.data.pickup_date = parseInt($prevSelectedDate[0]);
			$scope.data.pickup_month = parseInt($prevSelectedDate[1]);
			$scope.data.pickup_year = parseInt($prevSelectedDate[2]);
		}

		$scope.pickerDate = function($key) {
			var getCurrentDate = $scope.data.pickup_date;
			var getCurrentMonth = $scope.data.pickup_month;
			var getCurrentYear = $scope.data.pickup_year;
			if($key=='add') {
				if(getCurrentDate < daysInMonth(getCurrentMonth,getCurrentYear)) {
					var setMyDate = (getCurrentDate+1);
					$scope.data.pickup_date = setMyDate;
				}
			} else {
				if(getCurrentDate > 1) {
					var setMyDate = (getCurrentDate-1);
					$scope.data.pickup_date = setMyDate;
				}
			}
		}
		function checkLastDate(date,month,year) {
			if(date >= daysInMonth(month,year)) {
				var setMyDate = daysInMonth(month,year);
				$scope.data.pickup_date = setMyDate;
			}
		}
		$scope.pickerMonth = function($key) {
			var getCurrentDate = $scope.data.pickup_date;
			var getCurrentMonth = $scope.data.pickup_month;
			var getCurrentYear = $scope.data.pickup_year;
			if($key == 'add') {
				if(getCurrentMonth < 12) {
					var setMyMonth = (getCurrentMonth+1);
					$scope.data.pickup_month = setMyMonth;	
					checkLastDate(getCurrentDate,setMyMonth,getCurrentYear);
				}
			} else {
				if(getCurrentMonth > 1) {
					var setMyMonth = (getCurrentMonth-1);
					$scope.data.pickup_month = setMyMonth;
					checkLastDate(getCurrentDate,setMyMonth,getCurrentYear);
				}
			}

		}
		$scope.pickerYear = function($key) {
			var getCurrentDate = $scope.data.pickup_date;
			var getCurrentMonth = $scope.data.pickup_month;
			var getCurrentYear = $scope.data.pickup_year;
			if($key == 'add') {
				var setMyYear = (getCurrentYear+1);
				$scope.data.pickup_year = setMyYear;	
				checkLastDate(getCurrentDate,getCurrentMonth,setMyYear);

			} else {
				if(getCurrentYear != tomorrowYear) {
					var setMyYear = (getCurrentYear-1);
					$scope.data.pickup_year = setMyYear;
					checkLastDate(getCurrentDate,getCurrentMonth,setMyYear);
				}
			}
		}
		
		$template = '<div class="row">';
		$template += '<div class="col"><button on-hold="pickerDate('+"'add'"+')" ng-click="pickerDate('+"'add'"+')" class="button button-block button-positive icon ion-ios7-arrow-up"></button><input type="text" readonly ng-model="data.pickup_date"><button  ng-click="pickerDate('+"'minus'"+')" class="button button-block button-positive icon ion-ios7-arrow-down"></button></div>';
		$template += '<div class="col"><button ng-click="pickerMonth('+"'add'"+')" class="button button-block button-positive icon ion-ios7-arrow-up"></button><input type="text" readonly ng-model="data.pickup_month"><button ng-click="pickerMonth('+"'minus'"+')" class="button button-block button-positive icon ion-ios7-arrow-down"></button></div>';
		$template += '<div class="col"><button ng-click="pickerYear('+"'add'"+')" class="button button-block button-positive icon ion-ios7-arrow-up"></button><input type="text" readonly ng-model="data.pickup_year"><button ng-click="pickerYear('+"'minus'"+')" class="button button-block button-positive icon ion-ios7-arrow-down"></button></div>';
		$template += '</div>';
		  // An elaborate, custom popup
		  var myPopup = $ionicPopup.show({
			template: $template,
			title: 'Select Pick-up Date',
			scope: $scope,
			buttons: [
			  {
				text: '<b>Select Date</b>',
				type: 'button-positive',
				onTap: function(e) {
				  if (!$scope.data.pickup_date && !$scope.data.pickup_month && !$scope.data.pickup_year) {
					//don't allow the user to close unless he enters wifi password
					
					e.preventDefault();
				  } else {
					return $scope.data;
				  }
				}
			  },
			]
		  });
		  myPopup.then(function(res) {
			function putObject(path, object, value) {
			var modelPath = path.split(".");		
			function fill(object, elements, depth, value) {
				var hasNext = ((depth + 1) < elements.length);
				if(depth < elements.length && hasNext) {
					if(!object.hasOwnProperty(modelPath[depth])) {
						object[modelPath[depth]] = {};
					}
					fill(object[modelPath[depth]], elements, ++depth, value);
				} else {
					object[modelPath[depth]] = value;
				}
			}
			fill(object, modelPath, 0, value);
			}
			$selectedDate = res.pickup_date+"/"+res.pickup_month+"/"+res.pickup_year;
			putObject($model, $scope, $selectedDate);
		  });
		 };
	
