
      //  console.log('5');
        $scope.started = false;
        $location.path('/login');
       // console.log('6');
        //$location.path('/login');
        //console.log('7');
		/*Auth.logout();
		$interval.cancel(interval);
		$location.path('/logout');
		$timeout(function(){
			$location.path('/');
		},2000);*/
	}
	
	this.downloadDocs = function(){
		loginFactory.downloadDocs()
        .then(
        		function(data) {
        			var file= new Blob([data],{type : 'application/zip'});
        			saveAs(file,"BS_Help_Doc.zip");
        		},function(errReponse){
        			
        		});
	}
	
	function closeModals() { //2
        if ($scope.warning) {
            $scope.warning.close();
            $scope.warning = null;
        }
        if ($scope.timedout) {
            $scope.timedout.setEnabled = false;
            $scope.timedout.close();
            $scope.timedout = null;
        }
    }
    $scope.$on('IdleStart', function () {
        if(app.isUserLoggedIn)
        {	closeModals();
        $scope.warning = $modal.open({ templateUrl: 'warning-dialogSecurity.html', windowClass: 'modal-danger' });
        }
    });
    $scope.$on('IdleEnd', function ()
    { closeModals(); });
    $scope.$on('IdleTimeout', function () {
    	 if(app.isUserLoggedIn){
    		 showModal(2);
    			$interval.cancel(interval);
    		 closeModals();
    	        Idle.unwatch();
    	        $scope.started = false;
    	        $state.go('login');
    	 }
    	/*showModal(2);
		$interval.cancel(interval);
        //$window.sessionStorage.clear();
        //window.localStorage.clear();
        closeModals();
        Idle.unwatch();
        $scope.started = false;
        $location.path('/login');
        logoutFactory
        .logoutUser()
         .then(function (data) {
             closeModals();
             Idle.unwatch();
             $scope.started = false;
             $location.path('/login');
         })
        closeModals();*/
    });
    $scope.start = function () {
        closeModals();
        Idle.watch();
        $scope.started = true;
    }; $scope.stop = function () {
        closeModals();
        Idle.unwatch();
        $scope.started = false;
    };
});
