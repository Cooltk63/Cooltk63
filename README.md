app.factory('loginFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        fetchUser: fetchUser,
        reNewSession: reNewSession,
        downloadDocs: downloadDocs,

    };

    return factory;

    function fetchUser(user) {
        var REST_SERVICE_URI = './Security/login';
        var deferred = $q.defer();
        //console.log('Returned Id fetchUser : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function reNewSession(user) {
        var REST_SERVICE_URI = './Security/reNewSession';
        var deferred = $q.defer();
        // console.log('Returned Id reNewSession : ' + user.userId);
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadDocs() {
        var REST_SERVICE_URI = './Security/downloadDocs';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, null, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);
app.factory('CsrfTokenInterceptorService', ['$q',
    function CsrfTokenInterceptorService($q) {

        // Private constants.
        var CSRF_TOKEN_HEADER = 'X-CSRF-TOKEN',
            HTTP_TYPES_TO_ADD_TOKEN = ['DELETE', 'POST', 'PUT', 'GET'];

        // Private properties.
        /*
        var token=$('input[name="csrfPreventionSalt"]').val();
*/
        var token = document.getElementById("csrfPreventionSalt").value;


        // Public interface.
        var service = {
            response: onSuccess,
            responseError: onFailure,
            request: onRequest,
        };

        return service;

        // Private functions.
        function onFailure(response) {
            if (response.status === 403) {
                console.log('Request forbidden. Ensure CSRF token is sent for non-idempotent requests.');
            }

            return $q.reject(response);
        }

        function onRequest(config) {
            if (HTTP_TYPES_TO_ADD_TOKEN.indexOf(config.method.toUpperCase()) !== -1) {

                config.headers[CSRF_TOKEN_HEADER] = token;
                // config.setRequestHeader("csrfPreventionSalt",token);

            }

            return config;
        }

        function onSuccess(response) {

            var newToken = response.headers("csrfPreventionSalt");
            if (newToken) {
                token = newToken;
            }

            return response;
        }
    }]);

app.factory('pnlVarFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URL = './PnlVar/generateReport';

    var REST_SERVICE_URI2 = './PnlVar/downloadPnlReport';

    var factory = {
        downloadPnlReport: downloadPnlReport,
        generateReport: generateReport,
    }
    return factory;

    function generateReport(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, params).then(function (response) {
            //  console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {

            /*ISD Change*/
            /*console.error('Error');*/

            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadPnlReport(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI2, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {

            /*ISD Change*/
            /*  console.error('Error while updating User');*/

            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('compCodeDataFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_1 = './CompCodeReport/circleCodeUsList';
    var REST_SERVICE_URI_2 = './CompCodeReport/downloadXlReport';
    var REST_SERVICE_URI_3 = './CompCodeReport/branchCodeUsList';
    var REST_SERVICE_URI_4 = './CompCodeReport/getAssetCompList';
    var REST_SERVICE_URI_5 = './CompCodeReport/getExpenseCompList';
    var REST_SERVICE_URI_6 = './CompCodeReport/getIncomeCompList';
    var REST_SERVICE_URI_7 = './CompCodeReport/getLiabilityCompList';
    var REST_SERVICE_URI_8 = './CompCodeReport/downloadPdfReport';
    var REST_SERVICE_URI_9 = './CompCodeReport/getQedDate';
    var REST_SERVICE_URI_10 = './CompCodeReport/getSearchFlag';
    var REST_SERVICE_URL = './CompCodeReport/generateCompCodeReport';
    var REST_SERVICE_URL_1 = './CompCodeReport/addCompRow';

    var factory = {

        getCircleCodeUs: getCircleCodeUs,
        getAssetCompList: getAssetCompList,
        getExpenseCompList: getExpenseCompList,
        getIncomeCompList: getIncomeCompList,
        getLiabilityCompList: getLiabilityCompList,
        downloadXlReport: downloadXlReport,
        downloadPdfReport: downloadPdfReport,
        getlistOfQdate: getlistOfQdate,
        generateCompCodeReport: generateCompCodeReport,
        getSearchFlag: getSearchFlag,
        getBranchListUs: getBranchListUs

    }
    return factory;


    function downloadXlReport(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadPdfReport(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_8, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while downloading report');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getlistOfQdate(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function getCircleCodeUs(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getBranchListUs(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getAssetCompList(param) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, param).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getExpenseCompList(param) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, param).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getIncomeCompList(param) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, param).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getLiabilityCompList(param) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_7, param).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getSearchFlag(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_10, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while searching branch code' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function generateCompCodeReport(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, params).then(function (response) {
            //  console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);

/*
* @author VED
* factory function added for dicgc report data mapping
*  */
app.factory('dicgcReportFactory', ['$http', '$q', function ($http, $q) {
    /*declare uri's for mapping*/
    var REST_SERVICE_URI_1 = './dicgcReport/getScreenParam';
    var REST_SERVICE_URI_2 = './dicgcReport/getValue1';
    var REST_SERVICE_URI_3 = './dicgcReport/saveData';
    var REST_SERVICE_URI_4 = './dicgcReport/submitReport';
    var REST_SERVICE_URI_5 = './dicgcReport/rowId';
    var REST_SERVICE_URI_6 = './dicgcReport/saveRow';
    var REST_SERVICE_URI_7 = './dicgcReport/deleteRow';
    var REST_SERVICE_URI_8 = './dicgcReport/getSavedData';
    var REST_SERVICE_URI_9 = './dicgcReport/getSec3Val';

    var factory = {
        /*todo : write factory functions*/
        getlistOfScreenParam: getlistOfScreenParam,
        getValue1: getValue1,
        saveData: saveData,
        submit: submit,
        getRowId: getRowId,
        saveRow: saveRow,
        deleteRow: deleteRow,
        //editRow: editRow,
        getSec3Val: getSec3Val,
    }

    function getlistOfScreenParam(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getValue1(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            console.log('Response Success' + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Saving params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getData(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_8, params).then(function (response) {
            console.log('Response Success' + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Saving params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveData(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteRow(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_7, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getRowId(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submit(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSec3Val(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params for Section 3');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return factory;
}]);

app.factory('mntAsciiFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URL = './MntAscii/getQendDates';
    var REST_SERVICE_URL1 = './MntAscii/uploadFile';
    var REST_SERVICE_URL2 = './MntAscii/deleteAscii';
    var factory = {
        getQendDates: getQendDates,
        deleteAscii: deleteAscii,
        UploadFile: UploadFile,
    }
    return factory;

    function UploadFile(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while uploading file');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getQendDates(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteAscii(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}
]);

app.factory('asciiLoadFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URL = './IbgUserController/getFileCheck';
    var REST_SERVICE_URL_1 = './IbgUserController/getNewFileCheck';
    var REST_SERVICE_URL1 = './IbgUserController/getFilesData';
    var REST_SERVICE_URL2 = './IbgUserController/valiadateAsciiFile';
    var REST_SERVICE_URL3 = './IbgUserController/loadAsciiFile';
    var factory = {
        checkFiles: checkFiles,
        checkNewFiles: checkNewFiles,
        valiadateFiles: valiadateFiles,
        getFilesData: getFilesData,
        loadAsciiFile: loadAsciiFile,
    }
    return factory;

    function loadAsciiFile(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL3, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function valiadateFiles(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL2, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFilesData(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkFiles(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkNewFiles(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL_1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);
app.factory('GSTINReportFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getGSTINdata: getGSTINdata,
        getReportList: getReportList,
        viewReport: viewReport,
        getMappingData: getMappingData,
    };

    return factory;


    function getGSTINdata() {
        var REST_SERVICE_URI = './ReportGstin/getGSTINdata';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getReportList(copy) {
        var REST_SERVICE_URI = './ReportGstin/getReportList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './ReportGstin/viewReportJrxml';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            console.log("download Pdf usrService");
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while download Pdf');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getReportList(copy) {
        var REST_SERVICE_URI = './ReportGstin/getReportList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMappingData() {
        var REST_SERVICE_URI = './BranchGstin/getMappingData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);


app.factory('branchGstinFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_4 = './BranchGstin/uploadGSTIN';
    var REST_SERVICE_URI_5 = './BranchGstin/downloadErrorFile';
    var factory = {
        getBranchData: getBranchData,
        viewReportMapping: viewReportMapping,
        getGSTINData: getGSTINData,
        updateData: updateData,
        getMappingData: getMappingData,
        uploadFile: uploadFile,
        downloadErrorFile: downloadErrorFile,
    };

    return factory;


    function downloadErrorFile(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params, {
            /*responseType : 'arraybuffer'*/
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while downloadErrorFile');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReportMapping(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './BranchGstin/viewReportMapping';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            console.log("download Pdf usrService");
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while download Pdf');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function uploadFile(fd) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, fd, {
            transformRequest: angular.identity,
            headers: {
                'Content-Type': undefined
            }
        }).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            //  console.error('Error while uploading file');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMappingData() {
        var REST_SERVICE_URI = './BranchGstin/getMappingData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getBranchData(user) {
        var REST_SERVICE_URI = './BranchGstin/getBranchData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getGSTINData() {
        var REST_SERVICE_URI = './BranchGstin/getGSTINData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateData(params) {
        var REST_SERVICE_URI = './BranchGstin/updateData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            //   console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('AuditMasterFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        insertAudit: insertAudit,
        getAuditData: getAuditData,
        updateData: updateData,

    };
    return factory;

    function updateData(data) {
        var REST_SERVICE_URI = './AuditMaster/updateData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, data).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getAuditData() {
        var REST_SERVICE_URI = './AuditMaster/getAuditData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function insertAudit(list) {
        var REST_SERVICE_URI = './AuditMaster/insertAudit';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, list).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('AuditBranchFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_4 = './AuditMapping/uploadAuditGroup';
    var REST_SERVICE_URI_5 = './AuditMapping/downloadErrorFile';
    var factory = {
        getBranchData: getBranchData,
        viewReportMapping: viewReportMapping,
        getAuditGrpData: getAuditGrpData,
        updateData: updateData,
        getMappingData: getMappingData,
        uploadFile: uploadFile,
        downloadErrorFile: downloadErrorFile,
    };

    return factory;


    function downloadErrorFile(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params, {
            /*responseType : 'arraybuffer'*/
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while downloadErrorFile');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReportMapping(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './AuditMapping/viewReportMapping';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            console.log("download Pdf usrService");
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while download Pdf');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function uploadFile(fd) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, fd, {
            transformRequest: angular.identity,
            headers: {
                'Content-Type': undefined
            }
        }).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            //  console.error('Error while uploading file');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMappingData() {
        var REST_SERVICE_URI = './AuditMapping/getMappingData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getBranchData(user) {
        var REST_SERVICE_URI = './AuditMapping/getBranchData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getAuditGrpData() {
        var REST_SERVICE_URI = './AuditMapping/getAuditData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateData(params) {
        var REST_SERVICE_URI = './AuditMapping/updateData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            //   console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('AuditReportsFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getAuditdata: getAuditdata,
        getReportList: getReportList,
        viewReport: viewReport,
        getMappingData: getMappingData,
    };

    return factory;


    function getAuditdata() {
        var REST_SERVICE_URI = './AuditReports/getAuditdata';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getReportList(copy) {
        var REST_SERVICE_URI = './AuditReports/getReportList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './AuditReports/viewReportJrxml';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            console.log("download Pdf usrService");
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while download Pdf');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMappingData() {
        var REST_SERVICE_URI = './AuditMapping/getMappingData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);

app.factory('GSTINMasterFactory', ['$http', '$q', function ($http, $q) {


    var factory = {
        insertGSTIN: insertGSTIN,
        getData: getData,
        updateData: updateData,
        checkGSTIN: checkGSTIN,

    };
    return factory;


    function checkGSTIN(data) {
        var REST_SERVICE_URI = './GSTINMaster/checkGSTIN';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, data).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateData(data) {
        var REST_SERVICE_URI = './GSTINMaster/updateData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, data).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getData() {
        var REST_SERVICE_URI = './GSTINMaster/getData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function insertGSTIN(list) {
        var REST_SERVICE_URI = './GSTINMaster/insertGSTIN';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, list).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('uploadMocFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URL = './Maker/createTextFile';
    var REST_SERVICE_URL1 = './Maker/checkFileStatus';

    var factory = {
        mocSave: mocSave,
        checkFileStatus: checkFileStatus
    };
    return factory;


    function mocSave(param) {

        var deferred = $q.defer();
        //console.log('Returned Id mocSave : ' + user.userId);

        $http.post(REST_SERVICE_URL, param).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;


    }

    function checkFileStatus(param) {

        var deferred = $q.defer();
        //console.log('Returned Id checkFileStatus : ' + user.userId);

        $http.post(REST_SERVICE_URL1, param).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('---------Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;


    }
}]);

app.factory('interFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getListOfCircles: getListOfCircles,
        getSelectedListOfData: getSelectedListOfData,
        submitPostInterse: submitPostInterse,
        checkInterseStatus: checkInterseStatus,
        acceptReject: acceptReject
    };

    return factory;

    function getListOfCircles(params) {
        var REST_SERVICE_URI = './FRTMaker/getListOfCircles';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting getListOfCircles');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSelectedListOfData(params) {
        var REST_SERVICE_URI = './FRTMaker/getSelectedListOfData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting getSelectedListOfData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function submitPostInterse(params) {
        var REST_SERVICE_URI = './FRTMaker/submitPostInterse';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting submitPostInterse');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkInterseStatus(param) {
        var REST_SERVICE_URI = './FRTMaker/checkInterseStatus';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting checkInterseStatus');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    /*function getAlreadySelectedCirclesAndCompCode(param) {
        var REST_SERVICE_URI = './FRTMaker/getAlreadySelectedCirclesAndCompCode';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI,param).then(function(response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function(errResponse) {
            console.error('Error while getting checkInterseStatus');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }*/

    function acceptReject(param) {
        var REST_SERVICE_URI = './FRTMaker/acceptReject';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting acceptReject');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);


app.factory('updatePasswordFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getAuthUser: getAuthUser,
        getPropertiesFileData: getPropertiesFileData,
        updatePropertiesFileData: updatePropertiesFileData,
    };

    return factory;

    function getAuthUser(user) {
        var REST_SERVICE_URI = './Admin/getAuthUser';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getPropertiesFileData() {
        var REST_SERVICE_URI = './Admin/getPropertiesFileData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updatePropertiesFileData(params) {
        var REST_SERVICE_URI = './Admin/updatePropertiesFileData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('DWHFileFactory', ['$http', '$q', function ($http, $q) {
    var factory = {
        getLastAction: getLastAction,
        generateFile: generateFile,
        getUserAuth: getUserAuth,
        getActionCount: getActionCount,
    };
    return factory;

    function getLastAction(user) {

        var REST_SERVICE_URL = './DWH/getLastAction';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, user).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getActionCount(user) {

        var REST_SERVICE_URL = './DWH/getActionCount';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, user).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getUserAuth(user) {

        var REST_SERVICE_URL = './DWH/getUserAuth';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, user).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function generateFile(user) {

        var REST_SERVICE_URL = './DWH/generateFile';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);


app.factory('bsFooterFactory', ['$http', '$q', function ($http, $q) {
    var factory = {
        getFooter: getFooter,
        getBsSchedule: getBsSchedule,
        update: update,
        save: save,
        deleteFooter: deleteFooter
    };
    return factory;

    function save(data) {
        var REST_SERVICE_URL = './Admin/insertBsFooter';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, data).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function update(value) {
        var REST_SERVICE_URL = './Admin/submitBsFooter';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, value).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function deleteFooter(list) {
        var REST_SERVICE_URL = './Admin/deleteBsFooter';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, list).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getFooter() {
        var REST_SERVICE_URL = './Admin/getBsFooter';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getBsSchedule() {
        var REST_SERVICE_URL = './Admin/getBsSchedule';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);


app.factory('ConfigureSignFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getListOfSign: getListOfSign,
        getDetails: getDetails,
        getListOfSignPosition: getListOfSignPosition,
        save: save,
        delete1: delete1,
        getViewList: getViewList
    };


    function getDetails(param) {
        var REST_SERVICE_URI = './ConfigureSign/search';
        var deferred = $q.defer();
        console.log("Service-----");
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log("inside Service")
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function getViewList(param) {
        var REST_SERVICE_URI = './ConfigureSign/viewlist';
        var deferred = $q.defer();
        console.log("viewist service-----");
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log("inside view Service")
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function delete1(param) {
        var REST_SERVICE_URI = './ConfigureSign/delete';
        var deferred = $q.defer();
        console.log("Service---delete--");
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log("inside Service delete")
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function save(param) {
        var REST_SERVICE_URI = './ConfigureSign/save';
        var deferred = $q.defer();
        console.log("Service--save---");
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log("inside Service save")
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function getListOfSign() {
        var REST_SERVICE_URL1 = './ConfigureSign/getSign';
        var deferreed = $q.defer();
        /*alert("In service:::getListOfMapRPT");*/
        $http.post(REST_SERVICE_URL1).then(function (response) {
            console.log(response.data);
            deferreed.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching the data')
            deferreed.reject(errResponse);
        });
        return deferreed.promise;
    }

    function getListOfSignPosition() {
        var REST_SERVICE_URL1 = './ConfigureSign/getSignPos';
        var deferreed = $q.defer();
        /*alert("In service:::getListOfMapRPT");*/
        $http.post(REST_SERVICE_URL1).then(function (response) {
            console.log(response.data);
            deferreed.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching the data')
            deferreed.reject(errResponse);
        });
        return deferreed.promise;
    }

    return factory;
}]);


app.factory('adminLogFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './adminlog';
    var REST_SERVICE_URI_1 = './adminloginsertupdate';
    var factory = {
        mocCheck: mocCheck,
        mocInsertUpdate: mocInsertUpdate
    };
    return factory;

    function mocCheck(query) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, query).then(function (response) {

            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function mocInsertUpdate(query) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, query).then(function (response) {

            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);
app.factory('mocReversalFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/getPostedMocs';
    var REST_SERVICE_URI_1 = './Maker/reverseMocs';
    var REST_SERVICE_URI_2 = './Maker/deleteAllMocs';
    var factory = {
        getPostedMocs: getPostedMocs,
        reverseMocs: reverseMocs,
        deleteAllMocs: deleteAllMocs
    };

    return factory;

    function deleteAllMocs(params) {
        var deferred = $q.defer();

        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while deleteAllMocs');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getPostedMocs(user) {
        var deferred = $q.defer();
        //console.log('Returned Id getPostedMocs : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function reverseMocs(mocsList) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, mocsList).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while reversing MOCs');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);
app.factory('frtAdminFactory', ['$http', '$q', function ($http, $q) {
    var factory = {
        approveAdjustAmounts: approveAdjustAmounts,
        approvePrevAdjustAmounts: approvePrevAdjustAmounts,
        getWBSDataAdjust: getWBSDataAdjust,
        areAllCircleFreezed: areAllCircleFreezed,
		downloadEXLReport: downloadEXLReport
    };

    function approveAdjustAmounts(params) {
        var REST_SERVICE_URI = './FRTAdmin/approveAdjustAmounts';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function approvePrevAdjustAmounts(params) {
        var REST_SERVICE_URI = './FRTAdmin/approvePrevAdjustAmounts';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getWBSDataAdjust(user) {
        var REST_SERVICE_URI = './FRTAdmin/getWBSDataAdjust';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function areAllCircleFreezed(user) {
        var REST_SERVICE_URI = './FRTAdmin/areAllCircleFreezed';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

	function downloadEXLReport(params) {
		var REST_SERVICE_URI = './FRTAdmin/downloadEXLReport'; //2822107
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return factory;
}]);


app.factory('mocFreezedFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URL = './Maker/getList';
    var REST_SERVICE_URL1 = './Maker/IgnoreMoc';
    var REST_SERVICE_URL2 = './Maker/processMOC';
    var REST_SERVICE_URL3 = './Maker/updateMoc';
    var REST_SERVICE_URL4 = './Maker/modifyMoc';
    var REST_SERVICE_URL5 = './Maker/deleteMocdata';
    var REST_SERVICE_URL6 = './Maker/deleteInBS';

    var factory = {
        getMOC: getMOC,
        Ignore: Ignore,
        processMOC: processMOC,
        updateMoc: updateMoc,
        modifyMoc: modifyMoc,
        deleteMoc: deleteMoc,
        deleteInBS: deleteInBS,
    };
    return factory;

    function Ignore(copy) {

        var deferred = $q.defer();
        //console.log('Returned Id 1582' + user.userId);

        $http.post(REST_SERVICE_URL1, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;


    }

    function deleteInBS(copy) {

        var deferred = $q.defer();
        //console.log('Returned Id deleteInBS : ' + user.userId);

        $http.post(REST_SERVICE_URL6, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;


    }

    function deleteMoc(copy) {

        var deferred = $q.defer();
        //console.log('Returned Id deleteMoc : ' + user.userId);

        $http.post(REST_SERVICE_URL5, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;


    }

    function modifyMoc(copy) {

        var deferred = $q.defer();
        //console.log('Returned Id modifyMoc : ' + user.userId);

        $http.post(REST_SERVICE_URL4, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;


    }

    function updateMoc(copy) {

        var deferred = $q.defer();
        //console.log('Returned Id updateMoc : ' + user.userId);

        $http.post(REST_SERVICE_URL3, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function processMOC(copy) {

        var deferred = $q.defer();
        //console.log('Returned Id : processMOC : ' + user.userId);

        $http.post(REST_SERVICE_URL2, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getMOC(copy) {

        var deferred = $q.defer();
        //console.log('Returned Id getMOC : ' + user.userId);

        $http.post(REST_SERVICE_URL, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

}]);


app.factory('frtMakerFactory', ['$http', '$q', function ($http, $q) {
    var factory = {
        getWBSDataAdjust: getWBSDataAdjust,
        submitAdjustAmounts: submitAdjustAmounts,
        saveAdjustAmounts: saveAdjustAmounts,
        getFRTWorklist: getFRTWorklist
    };

    function getFRTWorklist(user) {
        var REST_SERVICE_URI = './FRTMaker/getFRTWorklist';
        var deferred = $q.defer();
        //console.log('Returned Id getFRTWorklist : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getWBSDataAdjust(user) {
        var REST_SERVICE_URI = './FRTMaker/getWBSDataAdjust';
        var deferred = $q.defer();
        //console.log('Returned Id getWBSDataAdjust : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitAdjustAmounts(params) {
        var REST_SERVICE_URI = './FRTMaker/submitAdjustAmounts';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveAdjustAmounts(params) {
        var REST_SERVICE_URI = './FRTMaker/saveAdjustAmounts';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saveAdjustAmounts');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return factory;
}]);

app.factory('frtPrevMakerFactory', ['$http', '$q', function ($http, $q) {
    var factory = {
        getWBSDataAdjust: getWBSDataAdjust,
        submitAdjustAmounts: submitAdjustAmounts,
        saveAdjustAmounts: saveAdjustAmounts,
        getFRTWorklist: getFRTWorklist,
		downloadEXLReport: downloadEXLReport
    };

    function getFRTWorklist(user) {
        var REST_SERVICE_URI = './FRTMaker/getFRTWorklist';
        var deferred = $q.defer();
        //console.log('Returned Id getFRTWorklist : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getWBSDataAdjust(user) {
        var REST_SERVICE_URI = './FRTMaker/getPrevWBSDataAdjust';
        var deferred = $q.defer();
        //console.log('Returned Id getWBSDataAdjust : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitAdjustAmounts(params) {
        var REST_SERVICE_URI = './FRTMaker/submitPrevAdjustAmounts';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveAdjustAmounts(params) {
        var REST_SERVICE_URI = './FRTMaker/savePrevAdjustAmount';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saveAdjustAmounts');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

	function downloadEXLReport(params) {
		var REST_SERVICE_URI = './FRTMaker/downloadEXLReport'; //2822107
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    return factory;
}]);

app.factory('mocMakerFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/getBranchList';
    var REST_SERVICE_URI_1 = './Maker/getCompCodeList';
    var REST_SERVICE_URI_2 = './Maker/getcglList';
    var REST_SERVICE_URI_3 = './Maker/saveMocEntries';

    var REST_SERVICE_URI_4 = './Maker/uploadFile';
    var REST_SERVICE_URI_5 = './Maker/downloadErrorFile';

    var factory = {
        getBranchList: getBranchList,
        getCompCodeList: getCompCodeList,
        getcglList: getcglList,
        savemocList: savemocList,
        getYSASuplHeads: getYSASuplHeads,
        downloadReport: downloadReport,
        getRejectedMocs: getRejectedMocs,
        getMocsForApproval: getMocsForApproval,
        accRejMoc: accRejMoc,
        submitEditMOC: submitEditMOC,
        deleteMOC: deleteMOC,
        getCompList: getCompList,
        uploadFile: uploadFile,
        downloadErrorFile: downloadErrorFile,
        getMocNumOfGroups: getMocNumOfGroups,
        callEis: callEis

    };

    return factory;

    function callEis(eisParam) {
        var REST_SERVICE_URI = './Maker/callEisService';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, eisParam).then(function (response) {
            console.log("****** eisResponse Success in userService Js");
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('********** Error while getting eisResponse in userService Js');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getBranchList(user) {
        var deferred = $q.defer();
        //console.log('Returned Id getBranchList : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCompCodeList() {
        var deferred = $q.defer();
        $http.get(REST_SERVICE_URI_1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getcglList(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function savemocList(params) {
        console.log('params ' + params);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getYSASuplHeads(user) {
        var REST_SERVICE_URI = './Maker/getYSASuplHeads';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(formData) {
        var REST_SERVICE_URI = './Maker/downloadReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getRejectedMocs(user) {
        var REST_SERVICE_URI = './Maker/getRejectedMocs';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMocsForApproval(user) {
        var REST_SERVICE_URI = './Maker/getMocsForApproval';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMocNumOfGroups(param) {
        var REST_SERVICE_URI = './Maker/getMocNumOfGroups';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function accRejMoc(params) {
        // console.log('params '+params);
        var REST_SERVICE_URI = './Maker/accRejMoc';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while accRejMoc userService Js');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function submitEditMOC(params) {
        console.log('params ' + params);
        var REST_SERVICE_URI = './Maker/submitEditMOC';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function deleteMOC(params) {
        console.log('params ' + params);
        var REST_SERVICE_URI = './Maker/deleteMOC';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getCompList(type) {
        console.log('type ' + type);
        var REST_SERVICE_URI = './Maker/getCompList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, type).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function downloadErrorFile(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params, {
            /*responseType : 'arraybuffer'*/
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while downloadErrorFile');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function uploadFile(fd) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, fd, {
            transformRequest: angular.identity,
            headers: {
                'Content-Type': undefined
            }
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while uploading file');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('CreatedReportsFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_1 = './CreatedReports/circleCodeUsList';
    var REST_SERVICE_URI_2 = './CreatedReports/getCreated';
    var REST_SERVICE_URI_3 = './CreatedReports/downloadReport';
    var REST_SERVICE_URI_4 = './CreatedReports/listOfQdate';

    var factory = {

        getCircleCodeUs: getCircleCodeUs,
        getCreated: getCreated,
        downloadReport: downloadReport,
        getlistOfQdate: getlistOfQdate

    };

    return factory;

    function downloadReport(formData) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCircleCodeUs(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getlistOfQdate() {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching date');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCreated(rowUserData) {


        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, rowUserData).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);
app.factory('viewReportFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Admin/getReportData';
    var factory = {
        getReportData: getReportData
    };

    return factory;

    function getReportData(user) {
        var deferred = $q.defer();
        // console.log('Returned Id getReportData : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function logoutUser() {
        var deferred = $q.defer();
        return deferred.promise;
    }
}]);

app.factory('sc09Factory', ['$http', '$q', function ($http, $q) {

    var factory = {
        saveSC03Row: saveSC03Row,
        getSavedSC03Data: getSavedSC03Data,
        deleteSC03MainRow: deleteSC03MainRow,
        saveSC03Report: saveSC03Report,
        SubmitSC09Report: SubmitSC09Report,
        getSC09ValiadationData: getSC09ValiadationData,
        getSC09ReportData: getSC09ReportData,
        getSFTPSC9Data:getSFTPSC9Data,
        getSC09SftpActualData:getSC09SftpActualData
    };

    return factory;

    // Get SFTP Data
    function getSC09SftpActualData(sftpparams) {
        var deferred = $q.defer();
        var REST_SERVICE_SFTP_URI = './sftp/getSC09SftpReportData';
        $http.post(REST_SERVICE_SFTP_URI, sftpparams).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while get getSC09SftpActualData Data Fetching');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    // Get SFTP Data
    function getSFTPSC9Data(sftpparams) {
        var deferred = $q.defer();
        var REST_SERVICE_SFTP_URI = './sftp/getAnnexureSftp';
        $http.post(REST_SERVICE_SFTP_URI, sftpparams).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while get SFTP SC09Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function saveSC03Row(type) {
        var REST_SERVICE_URI_4 = './Maker/saveSC03Row';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, type).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedSC03Data() {
        var REST_SERVICE_URI_5 = './Maker/getSavedSC03Data';
        var deferred = $q.defer();
        $http.get(REST_SERVICE_URI_5).then(function (response) {
            console.log(response);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteSC03MainRow(uniqueId) {
        var REST_SERVICE_URI_6 = './Maker/deleteSC03MainRow';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, uniqueId).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveSC03Report(params) {
        var REST_SERVICE_URI_7 = './Maker/saveSC03Report';
        console.log('sss' + params);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_7, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function SubmitSC09Report(copy) {
        var REST_SERVICE_URI_8 = './Maker/SubmitSC09Report';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_8, copy).then(function (response) {
            console.log(response);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 09');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSC09ValiadationData(row) {
        var REST_SERVICE_URI_9 = './Maker/getSC09ValiadationData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, row).then(function (response) {
            console.log(response);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSC09ReportData(userInfo) {

        var REST_SERVICE_URI = './Maker/getSC09ReportData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, userInfo).then(function (response) {
            console.log(response);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('postReportsFactory', ['$http', '$q', function ($http, $q) {
    var factory = {
        getWorkList: getWorkList,
        downloadReport: downloadReport,
        signReport: signReport,
        downloadSignedReport: downloadSignedReport,
        sendPdf: sendPdf,
        rejectReport: rejectReport,
        mocCheck: mocCheck,
        autoInsert: autoInsert,
        statusOfNil: statusOfNil,
        sftpOfFiles: sftpOfFiles,
        checkFreezedOrNot: checkFreezedOrNot
    };

    return factory;


    function sftpOfFiles(params) {
        var REST_SERVICE_URI = './autoInsert/sftpOfFiles';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while sftpOfFiles');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function statusOfNil(params) {
        var REST_SERVICE_URI = './autoInsert/statusOfNil';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function autoInsert(params) {
        var REST_SERVICE_URI = './autoInsert/nilReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while execution');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rejectReport(params) {
        var REST_SERVICE_URI_1 = './Auditor/rejectReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getWorkList(user) {
        var REST_SERVICE_URI_1 = './Admin/getWorklist';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function mocCheck(user) {
        var REST_SERVICE_URI_1 = './Admin/mocCheck';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function checkFreezedOrNot(params) {
        var REST_SERVICE_URI_1 = './Admin/checkFreezedOrNot';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while checkFreezedOrNot');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function sendPdf(user) {
        var REST_SERVICE_URI_1 = '/BS/displayPDF.jsp';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(params) {
        var REST_SERVICE_URI_1 = './Admin/viewReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadSignedReport(params) {
        var REST_SERVICE_URI_1 = './Auditor/downloadSignedReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function signReport(params) {
        var REST_SERVICE_URI_1 = './Auditor/getSignedReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

/*
app.factory('asciiLoadFactory', [ '$http', '$q', function($http, $q) {
    var REST_SERVICE_URL = './Maker/checkFiles';
    var REST_SERVICE_URL1 = './Maker/getFilesData';
    var factory={
    checkFiles:checkFiles,
        getFilesData:getFilesData,
    }
    return factory;
    function getFilesData(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL1, params).then(function(response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function(errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function checkFiles(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, params).then(function(response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function(errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);*/
app.factory('uploadfileFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Admin/uploadFile';
    var REST_SERVICE_URI_1 = './Admin/deleteAsciData';
    //var REST_SERVICE_URI = './Admin/checkFiles';

    var factory = {
        uploadFile: uploadFile,
        deleteAsciData: deleteAsciData

    };

    return factory;

    /*function checkFiles(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function(response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function(errResponse) {
            console.error('Error while statusOfNil');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }*/


    function uploadFile(fd) {

        console.log(fd);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, fd, {
            transformRequest: angular.identity,
            headers: {
                'Content-Type': undefined
            }
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while uploading file');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function deleteAsciData(fd) {


        var deferred = $q.defer();

        $http.post(REST_SERVICE_URI_1, fd, {
            transformRequest: angular.identity,
            headers: {
                'Content-Type': undefined
            }
        }).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while daleting  ASCII data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);
app.factory('circleMakerWorklist', ['$http', '$q', function ($http, $q) {

    var factory = {
        getCircleMakerWorklist: getCircleMakerWorklist,
        downloadReport: downloadReport
    };

    return factory;

    function getCircleMakerWorklist(user) {
        var REST_SERVICE_URI_1 = './Maker/getCircleMakerWorklist';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(report) {
        var REST_SERVICE_URI_1 = './Maker/viewReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);
app.factory('adminReportFactoryNew', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_1 = './Admin/getReportDataNew';

    var factory = {
        getSavedDataNew: getSavedDataNew
    };

    return factory;

    function getSavedDataNew(user) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('checkerHomeFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        downBranchList: downBranchList,
        approveBranchList: approveBranchList

    };

    return factory;

    function downBranchList(user) {
        var REST_SERVICE_URI_1 = './Admin/downBranchList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function approveBranchList(user) {
        var REST_SERVICE_URI_1 = './Admin/approveBranchList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);
app.factory('adminReportFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Admin/getCompCodeList';
    var REST_SERVICE_URI_1 = './Admin/saveRow';
    var REST_SERVICE_URI_2 = './Admin/getSavedData';
    var REST_SERVICE_URI_3 = './Admin/deleteRow';

    var factory = {
        getCompCodeList: getCompCodeList,
        saveRow: saveRow,
        getSavedData: getSavedData,
        deleteMainRow: deleteMainRow,
        getBranches: getBranches,
        saveBranch: saveBranch,
        getCheckBranches: getCheckBranches,
        mapAndUnMapBranches: mapAndUnMapBranches,
        addMap: addMap,
        getDetails: getDetails,
        getSh03Data: getSh03Data,
        getSh04Data: getSh04Data,
    };

    return factory;

    function getCompCodeList(type) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, type).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(type) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, type).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedData() {
        var deferred = $q.defer();
        $http.get(REST_SERVICE_URI_2).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteMainRow(uniqueId) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, uniqueId).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getBranches(circleCode) {
        var REST_SERVICE_URI = './Admin/getBranches';
        var deferred = $q.defer();
        alert(circleCode);
        $http.post(REST_SERVICE_URI, circleCode).then(function (response) {
            console.log(response.data);

            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCheckBranches(params) {
        var REST_SERVICE_URI = './Admin/getCheckBranches';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);

            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function mapAndUnMapBranches(params) {
        var REST_SERVICE_URI = './Admin/mapAndUnMapBranches';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);

            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function addMap(miscData) {
        var REST_SERVICE_URI = './Admin/addMap';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, miscData).then(function (response) {
            console.log(response.data);

            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDetails(compCode) {
        var REST_SERVICE_URI = './Admin/getDetailsFromCompCode';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, compCode).then(function (response) {
            console.log(response.data);

            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveBranch(qb, $scope) {

        var REST_SERVICE_URI = './Admin/saveBranch';
        var deferred = $q.defer();
        // alert("circle"+circle);
        $http.post(REST_SERVICE_URI, qb).then(function (response) {
            console.log(response.data);

            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSh03Data(circleCode) {
        var REST_SERVICE_URI = './Admin/getSh03Data';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, circleCode).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSh04Data(circleCode) {
        var REST_SERVICE_URI = './Admin/getSh04Data';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, circleCode).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);
app.factory('userManageFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_5 = './UserManage/createUser';

    var REST_SERVICE_URI_8 = './UserManage/circleCodeUsList';
    var REST_SERVICE_URI_9 = './UserManage/RoleUsList';
    var REST_SERVICE_URI_10 = './UserManage/getUserInfo';
    var REST_SERVICE_URI_11 = './UserManage/DeleteUser';
    var REST_SERVICE_URI_12 = './UserManage/confirmCreateUser';
    var REST_SERVICE_URI = './UserManage/updateUserDetails';
    var REST_SERVICE_URI_1 = './UserManage/checkUserDetails';
    var REST_SERVICE_URI_2 = './UserManage/deleteUser';

    var factory = {

        createUser: createUser,
        getCircleCodeUs: getCircleCodeUs,
        getRoleUs: getRoleUs,
        getUserInfo: getUserInfo,
        DeleteUser: DeleteUser,
        confirmCreateUser: confirmCreateUser,
        updateUser: updateUser,
        checkUserDetails: checkUserDetails,
        deleteUser: deleteUser

    };

    return factory;

    function getCircleCodeUs(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_8, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getRoleUs(role) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, role).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching role');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getUserInfo() {

        // alert("in ser");
        //console.log('Returned Id getUserInfo : '+user.userId);
        var deferred = $q.defer();
        $http.get(REST_SERVICE_URI_10).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function DeleteUser(userdata) {

        // alert("in delete user service js " +JSON.stringify(userdata));

        var deferred = $q.defer();
        //console.log('Returned Id DeleteUser : ' + user.userId);

        $http.post(REST_SERVICE_URI_11, userdata).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function confirmCreateUser(userdata) {

        // alert("in confirmCreateUser user service js "
        // +JSON.stringify(userdata));

        var deferred = $q.defer();
        ////console.log('Returned Id confirmCreateUser : ' + user.userId);

        $http.post(REST_SERVICE_URI_12, userdata).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function createUser(copy) {

        // alert("in user manage service js " +JSON.stringify(copy));

        var deferred = $q.defer();
        // console.log('Returned Id : '+user.userId);

        $http.post(REST_SERVICE_URI_5, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateUser(copy) {

        console.log("serv  JS ::" + copy);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while uploading file');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkUserDetails(user) {
        console.log("serv Jss ::" + user);
        var deferred = $q.defer();
        //console.log('Returned Id checkUserDetails : ' + user.userId);

        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            console.log(response.data.status);
            console.log("aaaa");
            console.log(response.data.circleCode);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching User details');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteUser(copy) {
        console.log("serv Jss ::" + copy);
        var deferred = $q.defer();
        //console.log('Returned Id deleteUser :' + user.userId);

        $http.post(REST_SERVICE_URI_2, copy).then(function (response) {
            console.log(response.data);
            console.log(response.data.status);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while deleting User details');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('bsmapFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        searchRecord: searchRecord,
        getListOfMapRPT: getListOfMapRPT,
        getListOfType: getListOfType,
        recordUpdateFunction: recordUpdateFunction
    };

    return factory;

    function recordUpdateFunction(editParameters) {
        var REST_SERVICE_URL = './BsMapTable/recordUpdate';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, editParameters).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function searchRecord(parameter) {
        var REST_SERVICE_URL = './BsMapTable/searchRecord';
        var deferred = $q.defer();
        /*alert("Inside user_service JS"+ parameter);*/
        $http.post(REST_SERVICE_URL, parameter).then(function (response) {
            /* alert('inside UserServicJs'+response.data);*/
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getListOfMapRPT() {
        var REST_SERVICE_URL1 = './BsMapTable/getRPT';
        var deferreed = $q.defer();
        /*alert("In service:::getListOfMapRPT");*/
        $http.post(REST_SERVICE_URL1).then(function (response) {
            console.log(response.data);
            deferreed.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching the data')
            deferreed.reject(errResponse);
        });
        return deferreed.promise;
    }

    function getListOfType() {
        var REST_SERVICE_URL1 = './BsMapTable/getType';
        var deferreed = $q.defer();
        $http.post(REST_SERVICE_URL1).then(function (response) {
            console.log(response.data);
            deferreed.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching the data')
            deferreed.reject(errResponse);
        });
        return deferreed.promise;
    }

}]);


app.factory('postMocFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getCircleList: getCircleList,
        bringResult: bringResult,
        insertYourMoc: insertYourMoc,
        deleteInsertedMoc: deleteInsertedMoc
    };

    return factory;

    function getCircleList() {
        var REST_SERVICE_URL = './BranchManagement/circleList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function bringResult(userParameter) {

        var REST_SERVICE_URL = './FRTMaker/postMoc';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, userParameter).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting branch details');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function insertYourMoc(insertParameter) {

        var REST_SERVICE_URL = './FRTMaker/insertMoc';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, insertParameter).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting branch details');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function deleteInsertedMoc(parameter) {

        var REST_SERVICE_URL = './FRTMaker/deleteInsertedMoc';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, parameter).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting branch details');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

}]);

app.factory('branchFactory', ['$http', '$q', function ($http, $q) {

    var factory = {

        getCircleList: getCircleList,
        getBranchDetail: getBranchDetail,
        getUpdateStatus: getUpdateStatus,
        checkBranch: checkBranch,
        insertBranch: insertBranch,
        deleteBranch: deleteBranch
    };

    return factory;

    function getCircleList() {
        var REST_SERVICE_URL = './BranchManagement/circleList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function getBranchDetail(sentParameters) {
        var REST_SERVICE_URL = './BranchManagement/branchDetails';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, sentParameters).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting branch details');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function getUpdateStatus(updateParameters) {
        var REST_SERVICE_URL = './BranchManagement/update';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, updateParameters).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating U data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkBranch(brCode) {
        var REST_SERVICE_URL = './BranchManagement/checkBranch';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, brCode).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while checking branch details');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteBranch(brCode) {
        var REST_SERVICE_URL = './BranchManagement/deleteBranch';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, brCode).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while deleting branch details');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function insertBranch(brParameters) {
        var REST_SERVICE_URL = './BranchManagement/insertBranch';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URL, brParameters).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting branch details');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('sc02Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_8 = './Maker/submitScrTwo';
    var REST_SERVICE_URI_9 = './Maker/getSavedDataTwo';
    var REST_SERVICE_URI_10 = './Maker/getDataTwo';
    var REST_SERVICE_URI_11 = './Maker/getPreDataTwo';
    var REST_SERVICE_URI_12 = './Maker/getCountCheck';

    var factory = {

        submitScrTwo: submitScrTwo,
        getSavedDataTwo: getSavedDataTwo,
        getDataTwo: getDataTwo,
        getPreDataTwo: getPreDataTwo,
        getCountCheck: getCountCheck

    };

    function submitScrTwo(copy) {
        // alert("US clCode"+circleCode);
        // alert("Us date"+quarterEndDate);

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_8, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCountCheck(report) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_12, report).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getCountCheck');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataTwo(report) {

        // alert("inside UserServicJs "+circleCod);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, report).then(function (response) {
            // alert('inside UserServicJs'+response.data);
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDataTwo(curYeaData) {

        // alert("inside UserServicJs "+circleCod);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_10, curYeaData).then(function (response) {
            // alert('inside UserServicJs'+response.data);
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getPreDataTwo(prev) {

        // alert("inside UserServicJs "+circleCod);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_11, prev).then(function (response) {
            // alert('inside UserServicJs'+response.data);
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return factory;
}]);
app.factory('ysaFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_2 = './YSA/getPaticulars';
    var REST_SERVICE_URI_3 = './YSA/submitYSA';
    var REST_SERVICE_URI_4 = './YSA/getYsaSuplData';
    var REST_SERVICE_URI_5 = './YSA/getPreRequisteHeaderAmount';

    var factory = {

        getPaticulars: getPaticulars,
        submitYSA: submitYSA,
        getYsaSuplData: getYsaSuplData,
        getPreRequisteHeaderAmount: getPreRequisteHeaderAmount
    };

    return factory;

    function getPaticulars() {
        // alert("in service get particulars");

        var deferred = $q.defer();
        $http.get(REST_SERVICE_URI_2).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitYSA(copy) {
        // alert("in service submit ysa");
        // alert("circle code "+copy.circleCode);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating ysa');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getYsaSuplData(params) {
        // alert("in service get ysa data");
        // alert("ccode"+circlecode);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            console.log("service ysa data " + JSON.stringify(response.data));
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting ysa data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getPreRequisteHeaderAmount(row) {
        // alert("in service getPreRequisteHeaderAmount ysa");
        // alert("circle code "+circleCode);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, row).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getPreRequisteHeaderAmount ysa');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('qrc1Factory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_2 = './QRC1/getlistpart';
    var REST_SERVICE_URI = './QRC1/submitqrc1';


    var factory = {

        getlistpart: getlistpart,
        Submitqrc1: Submitqrc1,
    };

    return factory;

    function getlistpart(row) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, row).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function Submitqrc1(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 10');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);

/*app.factory('qrc4Factory', [ '$http', '$q', function($http, $q) {

    var REST_SERVICE_URI_2 = './QRC4/getlistpart';
    var REST_SERVICE_URI = './QRC4/submitqrc4';


    var factory = {

        getlistpart : getlistpart,
        Submitqrc4:Submitqrc4,
    };

    return factory;

    function getlistpart(row) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2,row).then(function(response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function(errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function Submitqrc4(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function(response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function(errResponse) {
            console.error('Error while submitting schedule 10');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


} ]);*/

app.factory('qrc16Factory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_2 = './QRC16/getlistpart';
    var REST_SERVICE_URI = './QRC16/submitqrc16';


    var factory = {

        getlistpart: getlistpart,
        Submitqrc16: Submitqrc16,
    };

    return factory;

    function getlistpart(row) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, row).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function Submitqrc16(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 10');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);

app.factory('PNLReportFactory', ['$http', '$q', function ($http, $q) {


    var factory = {
        submitPLSUPReport: submitPLSUPReport,
        getMakerReportScreenDetails: getMakerReportScreenDetails,
        getPreRequisteHeaderAmount: getPreRequisteHeaderAmount
    };

    return factory;

    function submitPLSUPReport(copy) {
        var REST_SERVICE_URI = './Maker/submitPLSUPReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting PNL');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMakerReportScreenDetails(userInfo) {
        // alert(userInfo.circleCode);
        var REST_SERVICE_URI = './Maker/getMakerReportScreenDetails';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, userInfo).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getMakerReportScreenDetails');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }


    function getPreRequisteHeaderAmount(row) {
        var REST_SERVICE_URI_PRE = './Maker/getPreRequisteHeaderAmount';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_PRE, row).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getPreRequisteHeaderAmount PNL');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('reopenFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_1 = './UserManage/allReportsInfo';
    var REST_SERVICE_URI_2 = './UserManage/reopenCircleReports';
    var REST_SERVICE_URI_3 = './UserManage/checkAdjSubmit';

    var factory = {

        allReportsInfo: allReportsInfo,
        reopenCircleReports: reopenCircleReports,
        checkAdjSubmit: checkAdjSubmit

    };

    return factory;

    function allReportsInfo(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching reports info');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkAdjSubmit(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while checkAdjSubmit');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function reopenCircleReports(reportdata) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, reportdata).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function logoutUser() {
        var deferred = $q.defer();
        return deferred.promise;
    }

}]);

app
    .factory(
        'reportController3Factory',
        [
            '$http',
            '$q',
            function ($http, $q) {

                var factory = {
                    getPreStaReportlist: getPreStaReportlist,
                    viewReport: viewReport
                };

                return factory;

                function getPreStaReportlist() {
                    var REST_SERVICE_URI = './Admin/getPreStaReportlist';
                    var deferred = $q.defer();
                    $http
                        .post(REST_SERVICE_URI)
                        .then(
                            function (response) {
                                console.log(response.data);
                                deferred
                                    .resolve(response.data);
                            },
                            function (errResponse) {
                                console
                                    .error('Error ::in Userservice Js :fetching pre stage reports');
                                deferred
                                    .reject(errResponse);
                            });
                    return deferred.promise;
                }

                function viewReport(formData) {
                    // alert(formData.report.status);
                    console.log("qwerty");
                    console.log(formData.report);

                    var REST_SERVICE_URI = './Admin/viewReportPreReports';
                    var deferred = $q.defer();
                    $http.post(REST_SERVICE_URI, formData, {
                        responseType: 'arraybuffer'
                    }).then(function (response) {
                        console.log(response.data);
                        deferred.resolve(response.data);
                    }, function (errResponse) {
                        console.error('Error while updating User');
                        deferred.reject(errResponse);
                    });
                    return deferred.promise;
                }

            }]);

app.factory('shc01Factory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI = './Shc01/getCurrentData';
    var REST_SERVICE_URI_1 = './Shc01/getSavedData';
    var REST_SERVICE_URI_2 = './Shc01/submit';

    var REST_SERVICE_URI_3 = './Shc01/getCountCheck';

    var factory = {

        getCurrentData: getCurrentData,
        getSavedData: getSavedData,
        submit: submit,
        getCountCheck: getCountCheck
        // submitAfterEdit:submitAfterEdit

    };

    return factory;

    function getCountCheck(row1) {
        console.log(row1.circle);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, row1).then(function (response) {

            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getCountCheck data in Js');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCurrentData(row1) {
        console.log(row1.circle);
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, row1).then(function (response) {

            console.log("aaaaaaa");

            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting data in Js');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedData(row1) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {

            console.log("aaaaaaa");

            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting data in Js');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submit(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, copy).then(function (response) {

            console.log("aaaaaaa");

            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting data in Js');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('UploadfileFactory2', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Admin/UploadFile2';

    var factory = {
        UploadFile2: UploadFile2

    };

    return factory;

    function UploadFile2(params) {

        console.log("UploadFile2 JS :" + params);

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while uploading file');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory("userFactory", function ($window) {
    function set(userKey, data) {

        window.localStorage.setItem(userKey, JSON.stringify(data));
        // console.log("dattaaaaaaaaa "+data);
        // console.log("JSONnnn "+JSON.stringify(data));
    }

    function get(userKey) {
        // console.log("JSON
        // "+JSON.parse(window.localStorage.getItem(userKey)));
        // console.log("WWW "+window.localStorage.getItem(userKey));
        return JSON.parse(window.localStorage.getItem(userKey));
    }

    return {
        set: set,
        get: get
    }
});

app.factory('checkerWorklistFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getWorklist2: getWorklist2,
        viewReport: viewReport,
        downloadReport: downloadReport,
        rejectReport: rejectReport,
        acceptReport: acceptReport,
        checkIsCircleFreeze: checkIsCircleFreeze
    };

    return factory;

    function getWorklist2(user) {
        var REST_SERVICE_URI = './Admin/getWorklist2';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkIsCircleFreeze(user) {
        var REST_SERVICE_URI = './Admin/checkIsCircleFreeze';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while checking Circle Freeze or not');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/viewReport2';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/downloadReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function acceptReport(formData) {
        var REST_SERVICE_URI = './Admin/acceptReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rejectReport(formData) {
        var REST_SERVICE_URI = './Admin/rejectReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('userDetailsFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getUserDetails: getUserDetails,
        submitUserDetails: submitUserDetails,
        checkBrList: checkBrList
    };

    return factory;

    function checkBrList(user) {
        var REST_SERVICE_URI_9 = './Admin/checkBrList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while checking brList');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getUserDetails(user) {
        var REST_SERVICE_URI = './Security/getUserDetails';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting User details');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitUserDetails(user) {
        var REST_SERVICE_URI = './Security/submitUserDetails';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitUserDetails User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('downloadJrxmlsFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getListOfReports: getListOfReports,
        viewReport: viewReport,
        viewReportCircle: viewReportCircle,
        downloadReport: downloadReport,
        getBrList: getBrList
    };

    return factory;

    function getBrList(user) {

        var REST_SERVICE_URI = './Admin/getBrList';

        var deferred = $q.defer();
        //console.log('Returned Id getBrList : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while gettng branchList');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getListOfReports(user) {
        var REST_SERVICE_URI = './Admin/getListOfReports';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/viewReportJrxml';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReportCircle(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/viewReportJrxmlCircle';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/downloadReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

/** ***********************************added for dashboard**************** */

app.factory('dashboardFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getListOfTypes: getListOfTypes,
        downloadReports: downloadReports
    };

    return factory;

    function getListOfTypes(capacity) {
        var REST_SERVICE_URI = './dashboard/getDashboard';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, capacity).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReports(formData) {
        var REST_SERVICE_URI = './dashboard/downloadReports';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while downloadReports');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

/** ***********************************End of dashboard**************** */

app.factory('downloadAsciiFilesFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Admin/getBranchList';
    var REST_SERVICE_URI_1 = './Admin/downloadAscii';
    var factory = {
        getBranchList: getBranchList,
        downloadAscii: downloadAscii

    };

    return factory;

    function downloadAscii(formData) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while downloading Ascii in User serv JS ');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getBranchList(user) {
        var deferred = $q.defer();
        //console.log('Returned Id getBranchList : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('downloadFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getlistOfFiles: getlistOfFiles,
        fileDownload: fileDownload,
        bulkFileDownload: bulkFileDownload,
        getlistOfFolders: getlistOfFolders
    };

    function getlistOfFolders(param) {
        var REST_SERVICE_URI = './Admin/getlistOfFolders';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return factory;

    function getlistOfFiles(param) {
        var REST_SERVICE_URI = './Admin/getlistOfFiles';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function fileDownload(param) {
        var REST_SERVICE_URI = './Admin/fileDownload';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, param).then(function (response) {
            console.log(response);
            deferred.resolve(response);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function bulkFileDownload(param) {
        // alert(quarter);
        var REST_SERVICE_URI = './Admin/bulkFileDownload';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, param, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('sc10Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/submitTen';
    var REST_SERVICE_URI_1 = './Maker/getSavedDataTen';
    var REST_SERVICE_URI_2 = './Maker/getValidationDataTen';

    var factory = {
        submitTen: submitTen,
        getSavedDataTen: getSavedDataTen,
        getValidationDataTen: getValidationDataTen,

    };

    return factory;

    function submitTen(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 10');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataTen(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data sc10');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getValidationDataTen(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while validation data sc10');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);


/* ### SC09- Report ### */
app.factory('sc9cFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/submitNineC';
    var REST_SERVICE_URI_1 = './Maker/getSavedDataNineC';

    var factory = {
        submitNineC: submitNineC,
        getSavedDataNineC: getSavedDataNineC

    };

    return factory;

    function submitNineC(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 9c');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataNineC(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data sc 9C');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('SCH10Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/submitSCH10';
    var REST_SERVICE_URI_1 = './Maker/getSavedDataSCH10';
    //var REST_SERVICE_URI_2 = './Maker/getPreviousYearDataSCH10';

    var factory = {
        submitSCH10: submitSCH10,
        getSavedDataSCH10: getSavedDataSCH10,
        /*getPreviousYearDataSCH10 : getPreviousYearDataSCH10,
        getCurrYearFirstRowDataSCH10 : getCurrYearFirstRowDataSCH10*/
    };

    return factory;

    function submitSCH10(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 9B');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataSCH10(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data sc 9B');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getPreviousYearDataSCH10(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Previous year data sc 9B');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCurrYearFirstRowDataSCH10(row1) {
        var REST_SERVICE_URI_9 = './Maker/getCurrYearFirstRowDataSCH10';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, row1).then(function (response) {
            console.log(response);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);


app.factory('sc9bFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/submitNineB';
    var REST_SERVICE_URI_1 = './Maker/getSavedDataNineB';
    var REST_SERVICE_URI_2 = './Maker/getPreviousYearDataNineB';

    var factory = {
        submitNineB: submitNineB,
        getSavedDataNineB: getSavedDataNineB,
        getPreviousYearDataNineB: getPreviousYearDataNineB,
        getCurrYearFirstRowDataNineB: getCurrYearFirstRowDataNineB
    };

    return factory;

    function submitNineB(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 9B');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataNineB(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data sc 9B');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getPreviousYearDataNineB(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Previous year data sc 9B');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCurrYearFirstRowDataNineB(row1) {
        var REST_SERVICE_URI_9 = './Maker/getCurrYearFirstRowDataNineB';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_9, row1).then(function (response) {
            console.log(response);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('sc9aFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/submitNineA';
    var REST_SERVICE_URI_1 = './Maker/getSavedDataNineA';

    var factory = {
        submitNineA: submitNineA,
        getSavedDataNineA: getSavedDataNineA

    };

    return factory;

    function submitNineA(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting schedule 9A');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataNineA(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data sc 9A');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('ApropriatnFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './FRTMaker/submitApropriatn';
    var REST_SERVICE_URI_1 = './FRTMaker/getSavedDataApropriatn';

    var factory = {
        submitApropriatn: submitApropriatn,
        getSavedDataApropriatn: getSavedDataApropriatn,

    };

    return factory;

    function submitApropriatn(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting Apropriatn');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataApropriatn(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data Apropriatn');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('schedule4Factory', ['$http', '$q', function ($http, $q) {

    var factory = {
        submitSchedule4Report: submitSchedule4Report,
        getSchedule4ReportData: getSchedule4ReportData
    };

    return factory;

    function submitSchedule4Report(copy) {
        var REST_SERVICE_URI = './FRTMaker/submitSchedule4Report';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitSchedule4Report');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSchedule4ReportData(copy) {
        var REST_SERVICE_URI = './FRTMaker/getSchedule4ReportData';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            console.log(response);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getSchedule4ReportData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('sc9cMigrationFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/getCirclelist';
    var REST_SERVICE_URI_1 = './Maker/submitNineMig';
    var REST_SERVICE_URI_2 = './Maker/getSavedDataNineMig';
    var REST_SERVICE_URI_3 = './Maker/getSavedDataNineMigTwo';

    var factory = {
        getCirclelist: getCirclelist,
        submitNineMig: submitNineMig,
        getSavedDataNineMig: getSavedDataNineMig,
        getSavedDataNineMigTwo: getSavedDataNineMigTwo
    };

    return factory;

    function getCirclelist() {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting circle list');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitNineMig(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting 9c mig');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataNineMig(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting 9c mig');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataNineMigTwo(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting 9c mig');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory("fileFactory", function ($window) {
    var savedData = {};

    function set(data) {
        savedData = data;
    }

    function get() {
        return savedData;
    }

    return {
        set: set,
        get: get
    }
});

app.factory('refreshFactory', ['$state', '$rootScope',
    function ($state, $rootScope) {
        var factory = {
            backToState: backToState
        }

        return factory;

        function backToState() {

            var sessionUser = $rootScope.globals.currentUser;
            // console.log(sessionUser);

            if (sessionUser.capacity == '51') {
                $state.go('circle_maker.worklist');
            } else if (sessionUser.capacity == '61') {
                $state.go('frt_maker.worklist');
            }
        }
    }]);


app.factory('sc9SuplFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI = './Maker/submitNineSupl';
    var REST_SERVICE_URI_1 = './Maker/getSavedDataNineSupl';

    var factory = {
        submitNineSupl: submitNineSupl,
        getSavedDataNineSupl: getSavedDataNineSupl
    };

    return factory;

    function submitNineSupl(copy) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, copy).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting  sc 9 Supl');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedDataNineSupl(row1) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, row1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data sc 9 Supl');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);


app.factory('provisionFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_1 = './FRTMaker/submitProvision';
    var REST_SERVICE_URI_2 = './FRTMaker/getprovisionInfo';
    var REST_SERVICE_URI_3 = './FRTMaker/deleteProvision';

    var factory = {

        submitProvision: submitProvision,
        getprovisionInfo: getprovisionInfo,
        deleteProvision: deleteProvision


    };

    return factory;


    function submitProvision(copy) {

        // alert("in user manage service js " +JSON.stringify(copy));

        var deferred = $q.defer();
        //console.log('Returned Id submitProvision' + user.userId);

        $http.post(REST_SERVICE_URI_1, copy).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function getprovisionInfo(quarterEndDate) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, quarterEndDate).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching provision info');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function deleteProvision(quarterEndDate) {

        // alert("in user manage service js " +JSON.stringify(copy));

        var deferred = $q.defer();
        //console.log('Returned Id deleteProvision :' + user.userId);

        $http.post(REST_SERVICE_URI_3, quarterEndDate).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while creating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);

app.factory('branchwiseFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_51 = './branchwise/circleCodeUsList';
    var REST_SERVICE_URI_53 = './branchwise/downloadPdfReport';
    var REST_SERVICE_URI_52 = './branchwise/downloadXLReport';
    var REST_SERVICE_URI_54 = './branchwise/listOfQdate';

    var factory = {

        getCircleCodeUs: getCircleCodeUs,
        downloadXLReport: downloadXLReport,
        downloadPdfReport: downloadPdfReport,
        getlistOfQdate: getlistOfQdate

    };

    return factory;

    function getCircleCodeUs(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_51, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadXLReport(params) {
        console.log("we are in br download fnc usr_serv");
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_52, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadPdfReport(params) {
        console.log("we are in br download fnc usr_serv");
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_53, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getlistOfQdate() {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_54).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching date');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('mocReportFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_61 = './mocReport/circleCodeUsList';
    var REST_SERVICE_URI_63 = './mocReport/downloadReport';
    var REST_SERVICE_URI_62 = './mocReport/downloadEXLReport';
    var REST_SERVICE_URI_64 = './mocReport/listOfQdate';

    var factory = {

        getCircleCodeUs: getCircleCodeUs,
        downloadEXLReport: downloadEXLReport,
        downloadReport: downloadReport,
        getlistOfQdate: getlistOfQdate

    };

    return factory;

    function getCircleCodeUs(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_61, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadEXLReport(params) {
        console.log("we are in br download fnc usr_serv");
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_62, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(params) {
        console.log("we are in br download fnc usr_serv");
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_63, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getlistOfQdate() {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_64).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching date');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

// Maker Factory Method
app.factory('miscMakerWorklistFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getMiscMakerWorklist: getMiscMakerWorklist,
        downloadReport: downloadReport,
        RMLUpdate: RMLUpdate,
        downloadPdfReport: downloadPdfReport,
        downloadXLReport: downloadXLReport

    };

    return factory;

    function RMLUpdate(index) {
        var REST_SERVICE_URI_1 = './MiscWorklist/miscReportEntryInMasterTable';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, index).then(function (responce) {
            console.log("RMLUpdate Response Data is " + responce.data)
            deferred.resolve(responce.data);

        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getMiscMakerWorklist(user) {
        var REST_SERVICE_URI_1 = './MiscWorklist/getMiscMakerWorklist';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(report) {
        var REST_SERVICE_URI_1 = './MiscWorklist/viewReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log("Response Dataof URI ArrayBuffer" + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadPdfReport(report) {
        var REST_SERVICE_URI_1 = './MiscWorklist/downloadPdfReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log("Response Dataof URI ArrayBuffer" + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadXLReport(report) {
        var REST_SERVICE_URI_1 = './MiscWorklist/downloadXLReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log("Response Dataof URI ArrayBuffer" + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);


/*XXXXXXXXXXXXXXXXX   XXXXXXXXXXXXXXXXXXXXXXXXXX   XXXXXXXXXXXXXXXXXXXXXXX*/


app.factory('MisccheckerWorklistFactory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getWorklist2: getWorklist2,
        viewReport2: viewReport2,
        downloadReport: downloadReport,
        rejectReport: rejectReport,
        acceptReport: acceptReport,
        downloadPdfReport: downloadPdfReport,
        downloadXLReport: downloadXLReport
    };
    return factory;

    function getWorklist2(user) {
        var REST_SERVICE_URI = './MiscWorklist/getMiscMakerWorklist';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReport2(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './MiscWorklist/viewReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './MiscWorklist/downloadReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function acceptReport(formData) {
        console.log("Sending Data " + formData);
        var REST_SERVICE_URI = './MiscWorklist/acceptReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rejectReport(formData) {
        var REST_SERVICE_URI = './MiscWorklist/rejectReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function downloadPdfReport(report) {
        var REST_SERVICE_URI_1 = './MiscWorklist/downloadPdfReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log("Response Dataof URI ArrayBuffer" + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadXLReport(report) {
        var REST_SERVICE_URI_1 = './MiscWorklist/downloadXLReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log("Response Dataof URI ArrayBuffer" + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);


app.factory('miscReportFactory', ['$http', '$q', function ($http, $q) {
    function getReportList(user) {
        console.log('In getReportList');
        var REST_SERVICE_URI = './miscReport/getReportList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function insertRML(params) {
        var REST_SERVICE_URI_1 = './miscReport/createReportId';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function insertRMLForFR(params) {
        var REST_SERVICE_URI_6 = './miscReport/inserRmlFR';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(report) {
        var REST_SERVICE_URI_1 = './miscReport/viewReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadPdfReport(params) {
        var REST_SERVICE_URI_1 = './miscReport/downloadPdfReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadEXLReport(params) {
        var REST_SERVICE_URI_1 = './miscReport/downloadEXLReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            //console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function accRejReport(params) {
        var REST_SERVICE_URI_1 = './miscReport/acceptReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rejectReport(params) {
        var REST_SERVICE_URI_1 = './miscReport/rejectReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        getReportList: getReportList,
        insertRML: insertRML,
        downloadReport: downloadReport,
        downloadPdfReport: downloadPdfReport,
        downloadEXLReport: downloadEXLReport,
        accRejReport: accRejReport,
        rejectReport: rejectReport,
        insertRMLForFR:insertRMLForFR,
    };
}]);

//Annex2C Factory for Circle
app.factory('annex2CFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_1 = './Annexure2C/getSavedDataAnnex2C';
    var REST_SERVICE_URI_2 = './Annexure2C/saveData';
    var REST_SERVICE_URI_3 = './Annexure2C/submitData';
    var REST_SERVICE_URI_4 = './sftp/getAnnexureSftp';


    var factory = {
        submitData: submitData,
        getSavedDataAnnex2C: getSavedDataAnnex2C,
        saveData: saveData,
        getSFTPAnnex2CData:getSFTPAnnex2CData
    };
    return factory;

    // Getting Backend Data
    function getSavedDataAnnex2C(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching saved data Annex2C');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    // ***

    // Save Data
    function saveData(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Saving saved data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    // ***

    // Submit Data
    function submitData(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            //// console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting Annex2C');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    // ***

    // SFTP Data
    function getSFTPAnnex2CData(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getSFTPAnnex2CData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }



}]);


// Annex2C Factory for FRT
app.factory('annex2CFRTFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI ='./FRTAnnex2CReport/getAnnex2cList';
    var REST_SERVICE_URI_1 ='./FRTAnnex2CReport/getAnnex2cInputData';
    var REST_SERVICE_URI_2 ='./FRTAnnex2CReport/saveData' ;

    var factory = {
        getAnnex2cList:getAnnex2cList,
        getAnnex2cInputData:getAnnex2cInputData,
        saveData:saveData
    };
    return factory;

    function getAnnex2cList(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting getAnnex2cList');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getAnnex2cInputData(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting getAnnex2cYSA >>>>>>>>>');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }



    function saveData(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            // console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting getAnnex2cIBG Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);

app.factory('frReturnFactory', ['$http', '$q', function ($http, $q) {
    function getReportList(params) {
        console.log('In getFRReportList service js');
        var REST_SERVICE_URI = './frReturn/getFRReportList';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while retrieving the data.');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function setRmId(rmId) {
        console.log('In getFRReportList');
        var REST_SERVICE_URI = './frReturn/setRmId';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, rmId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(params){
        var REST_SERVICE_URI_1 = './frReturn/viewReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    function AcceptIndividual(params) {
        console.log('In getFRReportList');
        var REST_SERVICE_URI = './frReturn/AcceptIndividual';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rejectReport(params) {
        console.log('In getFRReportList');
        var REST_SERVICE_URI = './frReturn/RejectIndividual';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function acceptAll(params) {
        console.log('In getFRReportList');
        var REST_SERVICE_URI = './frReturn/acceptAll';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating ');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function submit(params) {
        console.log('In getFRReportList');
        var REST_SERVICE_URI = './frReturn/submit';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating ');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitIndividual(params) {
        console.log('In submitIndividual function');
        var REST_SERVICE_URI = './frReturn/submitIndividual';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating ');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkFreeze(params) {
        console.log('In submitIndividual function');
        var REST_SERVICE_URI = './frReturn/checkFreeze';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating ');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function criMarr(params) {
        console.log('In submitIndividual function');
        var REST_SERVICE_URI = './frReturn/criMarr';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating ');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitIRIS(params) {
        console.log('In getFRReportList');
        var REST_SERVICE_URI = './frReturn/generateIrisContent';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating ');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        getReportList: getReportList,
        setRmId: setRmId,
        downloadReport:downloadReport,
        AcceptIndividual:AcceptIndividual,
        rejectReport:rejectReport,
        acceptAll:acceptAll,
        submit:submit,
        submitIndividual:submitIndividual,
        criMarr:criMarr,
        checkFreeze:checkFreeze,
        submitIRIS:submitIRIS
    };
}]);

app.factory('FRCapAdFactory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FRCapAdFactory insertOnLoad');
        let REST_SERVICE_URI = './FRCapAd/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getCapAdData(reportId) {
        console.log('In FRCapAdFactory getCapAdData');
        let REST_SERVICE_URI = './FRCapAd/getCapAdData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getCapAdData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In FRCapAdFactory updateField');
        let REST_SERVICE_URI = './FRCapAd/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getCapAdData: getCapAdData,
        updateField: updateField
    };
}]);


// Author V1012297
app.factory('FRDiscAssetFactory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FRCapAdFactory insertOnLoad');
        let REST_SERVICE_URI = './FRDiscAssetQuality/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDiscAssetQualityCYData(reportId) {
        console.log('In FRCapAdFactory getCapAdData');
        let REST_SERVICE_URI = './FRDiscAssetQuality/getDiscAssetQualityCYData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getCapAdData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function SaveData(params) {
        console.log('In FRCapAdFactory updateField');
        let REST_SERVICE_URI = './FRDiscAssetQuality/saveData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getDiscAssetQualityCYData:getDiscAssetQualityCYData,
        SaveData:SaveData
    };
}]);

// Author V1012297
app.factory('SegmentsSecFactory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FRCapAdFactory insertOnLoad');
        let REST_SERVICE_URI = './SegSecond/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSegSecondData(reportId) {
        let REST_SERVICE_URI = './SegSecond/getSegSecondData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getCapAdData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function SaveData(params) {
        let REST_SERVICE_URI = './SegSecond/saveData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getSegSecondData:getSegSecondData,
        SaveData:SaveData
    };
}]);

// Author V1012297
app.factory('T51Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FRCapAdFactory insertOnLoad');
        let REST_SERVICE_URI = './T51/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT51Data(reportId) {
        let REST_SERVICE_URI = './T51/getT51Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getCapAdData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function SaveData(params) {
        let REST_SERVICE_URI = './T51/saveData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT51Data:getT51Data,
        SaveData:SaveData
    };
}]);

app.factory('FRRepTransFactory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FRRepTransFactory insertOnLoad');
        let REST_SERVICE_URI = './FRRepTrans/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFRRepTransData(reportId) {
        console.log('In FRRepTransFactory getFRRepTransData');
        let REST_SERVICE_URI = './FRRepTrans/getRepTransData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFRRepTransData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateCYValue(params) {
        console.log('In FRRepTransFactory updateCYValue');
        let REST_SERVICE_URI = './FRRepTrans/updateCYValue';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateCYValue');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getFRRepTransData: getFRRepTransData,
        updateCYValue: updateCYValue
    };
}]);

app.factory('SecWiseNPAFactory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In SecWiseNPAFactory insertOnLoad');
        let REST_SERVICE_URI = './SecWiseNPA/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSecWiseNPAData(reportId) {
        console.log('In SecWiseNPAFactory getSecWiseNPAData');
        let REST_SERVICE_URI = './SecWiseNPA/getSecWiseNPAData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getSecWiseNPAData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In SecWiseNPAFactory updateField');
        let REST_SERVICE_URI = './SecWiseNPA/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getSecWiseNPAData: getSecWiseNPAData,
        updateField: updateField
    };
}]);

app.factory('T30Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T30Factory insertOnLoad');
        let REST_SERVICE_URI = './T30/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT30Data(reportId) {
        console.log('In T30Factory getT30Data');
        let REST_SERVICE_URI = './T30/getT30Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT30Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T30Factory updateField');
        let REST_SERVICE_URI = './T30/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT30Data: getT30Data,
        updateField: updateField
    };
}]);

app.factory('T41Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T41Factory insertOnLoad');
        let REST_SERVICE_URI = './T41/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT41Data(reportId) {
        console.log('In T41Factory getT41Data');
        let REST_SERVICE_URI = './T41/getT41Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT41Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T41Factory updateField');
        let REST_SERVICE_URI = './T41/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT41Data: getT41Data,
        updateField: updateField
    };
}]);

app.factory('T63Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T63Factory insertOnLoad');
        let REST_SERVICE_URI = './T63/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT63Data(reportId) {
        console.log('In T63Factory getT63Data');
        let REST_SERVICE_URI = './T63/getT63Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT63Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T63Factory updateField');
        let REST_SERVICE_URI = './T63/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submit(params) {
        console.log('In T63Factory updateField');
        let REST_SERVICE_URI = './T63/submit';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT63Data: getT63Data,
        updateField: updateField,
        submit:submit
    };
}]);

app.factory('FR_CF6Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FR_CF6Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_CF6/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFR_CF6Data(reportId) {
        console.log('In FR_CF6Factory getFR_CF6Data');
        let REST_SERVICE_URI = './FR_CF6/getFR_CF6Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_CF6Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFR_CF6_PARTData(reportId) {
        console.log('In FR_CF6Factory getFR_CF6_PARTData');
        let REST_SERVICE_URI = './FR_CF6/getFR_CF6_PARTData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_CF6_PARTData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In FR_CF6Factory updateField');
        let REST_SERVICE_URI = './FR_CF6/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getFR_CF6Data: getFR_CF6Data,
        getFR_CF6_PARTData: getFR_CF6_PARTData,
        updateField: updateField
    };
}]);

app.factory('FR_CF7Factory', ['$http', '$q', function ($http, $q) {


    function insertOnLoad(params) {
        console.log('In FR_CF7Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_CF7/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFR_CF7Data(reportId) {
        console.log('In FR_CF7Factory getFR_CF7Data');
        let REST_SERVICE_URI = './FR_CF7/getFR_CF7Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_CF7Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    /*function getFR_CF7_PARTData(reportId) {
        console.log('In FR_CF7Factory getFR_CF7_PARTData');
        let REST_SERVICE_URI = './FR_CF7/getFR_CF7_PARTData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_CF7_PARTData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }*/

    function updateField(params) {
        console.log('In FR_CF7Factory updateField');
        let REST_SERVICE_URI = './FR_CF7/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getFR_CF7Data: getFR_CF7Data,
        /*getFR_CF7_PARTData: getFR_CF7_PARTData,*/
        updateField: updateField
    };
}]);


app.factory('T42Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T42Factory insertOnLoad');
        let REST_SERVICE_URI = './T42/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT42Data(reportId) {
        console.log('In T42Factory getT42Data');
        let REST_SERVICE_URI = './T42/getT42Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT42Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T42Factory updateField');
        let REST_SERVICE_URI = './T42/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT42Data: getT42Data,
        updateField: updateField
    };
}]);

app.factory('T71Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T71Factory insertOnLoad');
        let REST_SERVICE_URI = './T71/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT71Data(reportId) {
        console.log('In T71Factory getT71Data');
        let REST_SERVICE_URI = './T71/getT71Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT71Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T71Factory updateField');
        let REST_SERVICE_URI = './T71/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT71Data: getT71Data,
        updateField: updateField
    };
}]);

app.factory('SC14Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In SC14Factory insertOnLoad');
        let REST_SERVICE_URI = './SC14/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSC14Data(reportId) {
        console.log('In SC14Factory getSC14Data');
        let REST_SERVICE_URI = './SC14/getSC14Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getSC14Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In SC14Factory updateField');
        let REST_SERVICE_URI = './SC14/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getSC14Data: getSC14Data,
        updateField: updateField
    };
}]);

app.factory('SC10Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In SC10Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_SC10/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSC10Data(reportId) {
        console.log('In SC10Factory getSC10Data');
        let REST_SERVICE_URI = './FR_SC10/getSC10Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getSC10Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In SC10Factory updateField');
        let REST_SERVICE_URI = './FR_SC10/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getSC10Data : getSC10Data,
        updateField : updateField
    };
}]);

app.factory('FR_SC12Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In SC12Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_SC12/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSC12Data(params) {
        console.log('In SC12Factory getSC12Data');
        let REST_SERVICE_URI = './FR_SC12/getFR_SC12Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getSC12Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In SC12Factory updateField');
        let REST_SERVICE_URI = './FR_SC12/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getSC12Data : getSC12Data,
        updateField : updateField
    };
}]);

app.factory('FR_SC02Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FR_SC02Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_SC02/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFR_SC02Data(params) {
        console.log('In FR_SC02Factory getFR_SC02Data');
        let REST_SERVICE_URI = './FR_SC02/getFR_SC02Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_SC02Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In FR_SC02Factory updateField');
        let REST_SERVICE_URI = './FR_SC02/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function checkWnStatus(params) {
        console.log('Checking Status for Wn complition');
        let REST_SERVICE_URI = './FR_SC02/checkWnStatus';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI,params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getFR_SC02Data : getFR_SC02Data,
        updateField : updateField,
        checkWnStatus : checkWnStatus

    };
}]);



app.factory('T17Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T17Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_T17/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT17Data(params) {
        console.log('In T17Factory getT17Data');
        let REST_SERVICE_URI = './FR_T17/getT17Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT17Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T17Factory updateField');
        let REST_SERVICE_URI = './FR_T17/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT17Data : getT17Data,
        updateField : updateField
    };
}]);



app.factory('T23Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T23Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_T23/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function checkWnStatus(params) {
        console.log('Checking Status for Wn complition');
        let REST_SERVICE_URI = './FR_T23/checkWnStatus';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI,params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT23Data(params) {
        console.log('In T23Factory getT23Data');
        let REST_SERVICE_URI = './FR_T23/getT23Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT23Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T23Factory updateField');
        let REST_SERVICE_URI = './FR_T23/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function updateCalculations(params) {
        let REST_SERVICE_URI = './FR_T23/updateCalculations';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT23Data : getT23Data,
        updateField : updateField,
        updateCalculations:updateCalculations,
        checkWnStatus:checkWnStatus
    };
}]);


app.factory('FR_WN04Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FR_WN04Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_WN04/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function getValues(params) {
        console.log('In FR_WN04Factory getValues');
        let REST_SERVICE_URI = './FR_WN04/GetValues';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getValues');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In FR_WN04Factory updateField');
        let REST_SERVICE_URI = './FR_WN04/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    return {
        insertOnLoad: insertOnLoad,
        updateField: updateField,
        getValues: getValues
    };
}]);

app.factory('dtadtlFactory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In dtadtlFactory insertOnLoad');
        let REST_SERVICE_URI = './dtadtl/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getdtadtlData(params) {
        console.log('In dtadtlFactory getdtadtlData');
        let REST_SERVICE_URI = './dtadtl/getDTAData';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getdtadtlData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateDTAUsrIn(params) {
        console.log('In dtadtlFactory updateDTAUsrIn');
        let REST_SERVICE_URI = './dtadtl/updateDTAUsrIn';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function updateCalculations(params) {
        let REST_SERVICE_URI = './dtadtl/updateCalculations';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getdtadtlData : getdtadtlData,
        updateDTAUsrIn : updateDTAUsrIn,
        updateCalculations:updateCalculations
    };
}]);

app.factory('FR_CashProFactory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In FR_CashProFactory insertOnLoad');
        let REST_SERVICE_URI = './FR_CashPro/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDetails(params) {
        console.log('In FR_CashProFactory getDetails');
        let REST_SERVICE_URI = './FR_CashPro/getDetails';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getDetails');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateFieldNEW(params) {
        console.log('In FR_CashProFactory updateFieldNEW');
        let REST_SERVICE_URI = './FR_CashPro/updateFieldNEW';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateFieldNEW');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getDetails : getDetails,
        updateFieldNEW: updateFieldNEW
    };
}]);

app.factory('T44Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T44Factory insertOnLoad');
        let REST_SERVICE_URI = './T44/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT44Data(reportId) {
        console.log('In T44Factory getT44Data');
        let REST_SERVICE_URI = './T44/getT44Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT44Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T44Factory updateField');
        let REST_SERVICE_URI = './T44/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT44Data: getT44Data,
        updateField: updateField
    };
}]);


app.factory('T01Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T01Factory insertOnLoad');
        let REST_SERVICE_URI = './T01/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT01Data(params) {
        console.log('In T01Factory getT01Data');
        let REST_SERVICE_URI = './T01/getT01Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT01Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T01Factory updateField');
        let REST_SERVICE_URI = './T01/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT01Data : getT01Data,
        updateField : updateField
    };
}]);

app.factory('T45Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T45Factory insertOnLoad');
        let REST_SERVICE_URI = './T45/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT45Data(reportId) {
        console.log('In T45Factory getT45Data');
        let REST_SERVICE_URI = './T45/getT45Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT45Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T45Factory updateField');
        let REST_SERVICE_URI = './T45/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT45Data: getT45Data,
        updateField: updateField
    };
}]);

app.factory('T48Factory', ['$http', '$q', function($http, $q) {

    function insertOnLoad(params) {
        console.log('In T48Factory insertOnLoad');
        let REST_SERVICE_URI = './T48/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT48Data(params) {
        console.log('In T48Factory getT48Data');
        let REST_SERVICE_URI = './T48/getT48Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT48Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T48Factory updateField');
        let REST_SERVICE_URI = './T48/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT48Data: getT48Data,
        updateField: updateField
    };
}]);

app.factory('T66Factory', ['$http', '$q', function($http, $q) {



    function insertOnLoad(params) {
        console.log('In T66Factory insertOnLoad');
        let REST_SERVICE_URI = './T66/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT66Data(params) {
        console.log('In T66Factory getT66Data');
        let REST_SERVICE_URI = './T66/getT66Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT48Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T66Factory updateField');
        let REST_SERVICE_URI = './T66/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        console.log('In T66Factory saveRow');
        let REST_SERVICE_URI = './T66/saveRow';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saveRow');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT66Data: getT66Data,
        updateField: updateField,
        saveRow: saveRow
    };
}]);

app.factory('T31Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T31Factory insertOnLoad');
        let REST_SERVICE_URI = './T31/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT31Data(reportId) {
        console.log('In T31Factory getT31Data');
        let REST_SERVICE_URI = './T31/getT31Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT31Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T31Factory updateField');
        let REST_SERVICE_URI = './T31/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT31Data : getT31Data,
        updateField : updateField
    };
}]);


app.factory('T49Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T49Factory insertOnLoad');
        let REST_SERVICE_URI = './T49/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT49Data(reportId) {
        console.log('In T49Factory getT49Data');
        let REST_SERVICE_URI = './T49/getT49Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT49Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T49Factory updateField');
        let REST_SERVICE_URI = './T49/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT49Data : getT49Data,
        updateField : updateField
    };
}]);

app.factory('T53Factory', ['$http', '$q', function ($http, $q) {
    function insertOnLoad(params) {
        console.log('In T53Factory insertOnLoad');
        let REST_SERVICE_URI = './T53/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT53Data(reportId) {
        console.log('In T53Factory getT53Data');
        let REST_SERVICE_URI = './T53/getT53Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT53Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T53Factory updateField');
        let REST_SERVICE_URI = './T53/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT53Data : getT53Data,
        updateField : updateField
    };
}]);

app.factory('T38Factory', ['$http', '$q', function($http, $q) {

    function insertOnLoad(params) {
        console.log('In T38Factory insertOnLoad');
        let REST_SERVICE_URI = './T38/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT38Data(params) {
        console.log('In T38Factory getT38Data');
        let REST_SERVICE_URI = './T38/getT38Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT38Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In T38Factory updateField');
        let REST_SERVICE_URI = './T38/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT38Data: getT38Data,
        updateField: updateField
    };
}]);

app.factory('FR_SC08Factory', ['$http', '$q', function($http, $q) {

    function insertOnLoad(params) {
        console.log('In FR_SC09Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_SC08/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFR_SC08Data(params) {
        console.log('In FR_SC08Factory getFR_SC08Data');
        let REST_SERVICE_URI = './FR_SC08/getFR_SC08Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_SC08Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In FR_SC08Factory updateField');
        let REST_SERVICE_URI = './FR_SC08/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getFR_SC08Data: getFR_SC08Data,
        updateField: updateField
    };
}]);

app.factory('FR_SC09Factory', ['$http', '$q', function($http, $q) {

    function insertOnLoad(params) {
        console.log('In FR_SC09Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_SC09/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFR_SC09Data(params) {
        console.log('In FR_SC09Factory getFR_SC09Data');
        let REST_SERVICE_URI = './FR_SC09/getFR_SC09Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_SC09Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In FR_SC09Factory updateField');
        let REST_SERVICE_URI = './FR_SC09/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getFR_SC09Data: getFR_SC09Data,
        updateField: updateField
    };
}]);

app.factory('FR_SC16Factory', ['$http', '$q', function($http, $q) {

    function insertOnLoad(params) {
        console.log('In FR_SC16Factory insertOnLoad');
        let REST_SERVICE_URI = './FR_SC16/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getFR_SC16Data(params) {
        console.log('In FR_SC16Factory getFR_SC16Data');
        let REST_SERVICE_URI = './FR_SC16/getFR_SC16Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getFR_SC16Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        console.log('In FR_SC16Factory updateField');
        let REST_SERVICE_URI = './FR_SC16/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getFR_SC16Data: getFR_SC16Data,
        updateField: updateField
    };
}]);

app.factory('CommonTxtFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_DETAILS = './CommonTxt/GetDetails';
    var REST_SERVICE_URI_SAVE = './CommonTxt/SaveDetails';
    var REST_SERVICE_URI_INSERT = './CommonTxt/insertOnLoad';

    return {
        getDetails: getDetails,
        saveData: saveData,
        insertOnLoad: insertOnLoad
    };

    function getDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_DETAILS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching data from backend');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveData(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function insertOnLoad(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('InvestmentFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './Inv/Insert';
    var REST_SERVICE_URI_UPDATE = './Inv/Update';
    var REST_SERVICE_URI_GET = './Inv/GetValues';


    return {
        insertOnload: insertOnload,
        updateField: updateField,
        getValues: getValues
    };

    function insertOnload(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function updateField(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_UPDATE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getValues(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_GET, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while displaying data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);


app.factory('SegmentsFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './Sgm/Insert';
    var REST_SERVICE_URI_SAVE = './Sgm/Save';
    var REST_SERVICE_URI_GETVALUES = './Sgm/GetValues';


    return {
        onLoadDetails: onLoadDetails,
        saveAmt: saveAmt,
        getValues: getValues
    };

    function onLoadDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveAmt(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getValues(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_GETVALUES, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);


app.factory('AssetQualityFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_ONLOAD = './AsstQlty/Onload';
    var REST_SERVICE_URI_SAVE = './AsstQlty/Save';
    var REST_SERVICE_URI_GET = './AsstQlty/Get';


    return {
        onLoadDetails: onLoadDetails,
        save: save,
        getValues: getValues
    };

    function onLoadDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_ONLOAD, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function save(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getValues(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_GET, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('FR_T3Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './T03/Insert';
    var REST_SERVICE_URI_SAVE = './T03/Save';
    var REST_SERVICE_URI_GET = './T03/GetValues';


    return {
        onLoadDetails: onLoadDetails,
        saveAmt: saveAmt,
        getValues: getValues
    };

    function onLoadDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveAmt(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getValues(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_GET, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);



app.factory('T02Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT= './T02/Insert';
    var REST_SERVICE_URI_SAVE= './T02/Save';
    var REST_SERVICE_URI_GET= './T02/GetValues';

    return {
        insertOnload :insertOnLoad,
        saveAmt :saveAmt,
        getValues:getValues
    };
    function insertOnLoad(params) {
        console.log('In T02Factory insertOnLoad');
        let REST_SERVICE_URI = './T02/Insert';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function saveAmt(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function getValues(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_GET, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while displaying data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);



app.factory('T56Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './T56/Insert';
    var REST_SERVICE_URI_SAVE = './T56/Save';
    var REST_SERVICE_URI_GET = './T56/GetValues';

    return {
        insertOnload: insertOnload,
        saveAmt: saveAmt,
        getValues: getValues
    };

    function insertOnload(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while inserting data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveAmt(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getValues(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_GET, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while displaying data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);


app.factory('RelPartFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './RelPart/insertOnLoad';
    var REST_SERVICE_URI_DETAILS = './RelPart/GetDetails';
    var REST_SERVICE_URI_SAVE = './RelPart/SaveDetails';

    return {
        insertOnLoad: insertOnLoad,
        getDetails: getDetails,
        saveVal: saveVal
    };

    function insertOnLoad(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_DETAILS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching data from backend');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveVal(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('ExpConRskFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './ExpConRsk/insertOnLoad';
    var REST_SERVICE_URI_DETAILS = './ExpConRsk/GetDetails';
    var REST_SERVICE_URI_SAVE = './ExpConRsk/SaveDetails';

    return {
        insertOnLoad: insertOnLoad,
        getDetails: getDetails,
        saveVal: saveVal
    };

    function insertOnLoad(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_DETAILS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching data from backend');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveVal(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('DetUnsecAdvFactory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './DetUnsecAdv/insertOnLoad';
    var REST_SERVICE_URI_DETAILS = './DetUnsecAdv/GetDetails';
    var REST_SERVICE_URI_SAVE = './DetUnsecAdv/SaveDetails';

    return {
        insertOnLoad: insertOnLoad,
        getDetails: getDetails,
        saveVal: saveVal
    };

    function insertOnLoad(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_DETAILS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching data from backend');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveVal(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('T47Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './T47/insertOnLoad';
    var REST_SERVICE_URI_DETAILS = './T47/GetDetails';
    var REST_SERVICE_URI_SAVE = './T47/SaveDetails';

    return {
        insertOnLoad: insertOnLoad,
        getDetails: getDetails,
        saveVal: saveVal
    };

    function insertOnLoad(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_DETAILS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching data from backend');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveVal(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('T29Factory', ['$http', '$q', function ($http, $q) {
    var REST_SERVICE_URI_INSERT = './T29/insertOnLoad';
    var REST_SERVICE_URI_DETAILS = './T29/GetDetails';
    var REST_SERVICE_URI_SAVE = './T29/SaveDetails';

    return {
        insertOnLoad: insertOnLoad,
        getDetails: getDetails,
        saveVal: saveVal
    };

    function insertOnLoad(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_INSERT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_DETAILS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching data from backend');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveVal(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_SAVE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saving data ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('tranche1Factory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_71 = './tranche1/index';
    var REST_SERVICE_URI_72 = './tranche1/edit';
    var REST_SERVICE_URI_7 = './tranche1/getAdditionalAmount';
    var REST_SERVICE_URI_73 = './tranche1/SubmitdcTReport';
    var REST_SERVICE_URI_74 = './tranche1/listOfParameters';
    var REST_SERVICE_URI_75 = './tranche1/getListofAmount';
    var REST_SERVICE_URI_76 = './tranche1/createOrEdit';
    var REST_SERVICE_URI_77 = './tranche1/reset';
    return {

        listOfParameters: listOfParameters,
        getListofAmount: getListofAmount,
        getAmount: getAmount,
        getAdditionalAmount: getAdditionalAmount,
        submitandsave: submitandsave,
        getAmountOnEdit: getAmountOnEdit,
        doCreateOrEdit: doCreateOrEdit,
        getAmountOnReset: getAmountOnReset,
    };

    function getListofAmount(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_75, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getAdditionalAmount() {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_7).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching additional amount');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function listOfParameters(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_74, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function doCreateOrEdit(params) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_76, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting status');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function getAmount(params2) {

        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_71, params2).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function submitandsave(params1) {
        console.log("into save*****");
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_73, params1).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getAmountOnEdit(params3) {
        console.log("into save*****");
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_72, params3).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getAmountOnReset(params) {
        console.log("into save*****");
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_77, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


}]);

app.factory('T36Factory', ['$http', '$q', function ($http, $q) {
    /*declare uri's for mapping*/
    const T36_GET_ROW_ID = './T36/rowId';
    const T36_SAVE_ROW = './T36/saveRow';
    const T36_DELETE_ROW = './T36/deleteRow';
    const T36_INSERT_RML = './T36/insertRml';
    const T36_GET_RML = './T36/getRml';
    const T36_SAVED_DATA = './T36/getSavedData';
    return {
        getReportId: getReportId,
        saveRow:saveRow,
        getRml:getRml,
        rmlEntry:rmlEntry,
        deleteRow:deleteRow,
        getSavedData:getSavedData
    }

    function getReportId() {
        var deferred = $q.defer();
        $http.post(T36_GET_ROW_ID).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T36 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rmlEntry(params) {
        var deferred = $q.defer();
        $http.post(T36_INSERT_RML, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T36 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getRml(params) {
        var deferred = $q.defer();
        $http.post(T36_GET_RML, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T36 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        var deferred = $q.defer();
        $http.post(T36_SAVE_ROW, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteRow(params) {
        var deferred = $q.defer();
        $http.post(T36_DELETE_ROW, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedData(params) {
        var deferred = $q.defer();
        $http.post(T36_SAVED_DATA, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params for saved rows');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('T62Factory', ['$http', '$q', function ($http, $q) {
    /*declare uri's for mapping*/
    const T62_GET_ROW_ID = './T62/rowId';
    const T62_SAVE_ROW = './T62/saveRow';
    const T62_DELETE_ROW = './T62/deleteRow';
    const T62_INSERT_RML = './T62/insertRml';
    const T62_GET_RML = './T62/getRml';
    const T62_SAVED_DATA = './T62/getSavedData';
    return {
        getReportId: getReportId,
        saveRow:saveRow,
        getRml:getRml,
        rmlEntry:rmlEntry,
        deleteRow:deleteRow,
        getSavedData:getSavedData
    }

    function getReportId() {
        var deferred = $q.defer();
        $http.post(T62_GET_ROW_ID).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T62 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rmlEntry(params) {
        var deferred = $q.defer();
        $http.post(T62_INSERT_RML, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T62 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getRml(params) {
        var deferred = $q.defer();
        $http.post(T62_GET_RML, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T62 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        var deferred = $q.defer();
        $http.post(T62_SAVE_ROW, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteRow(params) {
        var deferred = $q.defer();
        $http.post(T62_DELETE_ROW, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedData(params) {
        var deferred = $q.defer();
        $http.post(T62_SAVED_DATA, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params for saved rows');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('T68Factory', ['$http', '$q', function ($http, $q) {
    /*declare uri's for mapping*/
    const T68_GET_ROW_ID = './T68/rowId';
    const T68_SAVE_ROW = './T68/saveRow';
    const T68_DELETE_ROW = './T68/deleteRow';
    const T68_INSERT_RML = './T68/insertRml';
    const T68_GET_RML = './T68/getRml';
    const T68_SAVED_DATA = './T68/getSavedData';
    return {
        getReportId: getReportId,
        saveRow:saveRow,
        getRml:getRml,
        rmlEntry:rmlEntry,
        deleteRow:deleteRow,
        getSavedData:getSavedData
    }

    function getReportId() {
        var deferred = $q.defer();
        $http.post(T68_GET_ROW_ID).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T68 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rmlEntry(params) {
        var deferred = $q.defer();
        $http.post(T68_INSERT_RML, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T68 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getRml(params) {
        var deferred = $q.defer();
        $http.post(T68_GET_RML, params).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T68 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        var deferred = $q.defer();
        $http.post(T68_SAVE_ROW, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteRow(params) {
        var deferred = $q.defer();
        $http.post(T68_DELETE_ROW, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSavedData(params) {
        var deferred = $q.defer();
        $http.post(T68_SAVED_DATA, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params for saved rows');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

////Author by shilpa----------------------------------
app.factory('T57Factory', ['$http', '$q', function ($http, $q) {

    function insertOnLoad(params) {
        console.log('In T57Factory insertOnLoad');
        let REST_SERVICE_URI = './T57/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getT57Data(reportId) {
        console.log('In T57Factory getT57Data');
        let REST_SERVICE_URI = './T57/getT57Data';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getT57Data');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function updateField(params) {
        console.log('In T57Factory updateField');
        let REST_SERVICE_URI = './T57/updateField';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        console.log('In T57Factory saveRow');
        let REST_SERVICE_URI = './T57/saveRow';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while saveRow');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getT57Data : getT57Data,
        updateField : updateField,
        saveRow : saveRow
    };
}]);

////Author by shilpa---------commm//////////////////////


app.factory('commonFRAAddRowFactory', ['$http', '$q', function ($http, $q) {
    /!*declare uri's for mapping*!/
    var REST_SERVICE_URI_0 = './frAddRow/insertOnLoad';
    var REST_SERVICE_URI_1 = './frAddRow/getScreenParam';
    var REST_SERVICE_URI_2 = './frAddRow/saveRow';
    var REST_SERVICE_URI_3 = './frAddRow/deleteRow';
    var REST_SERVICE_URI_4 = './frAddRow/editRow';
    var REST_SERVICE_URI_5 = './frAddRow/getRowId';
    var REST_SERVICE_URI_6 = './frAddRow/frAddRow';


    function getReportId() {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting reportId for T36 :' +errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function getRowId(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function insertOnLoad(params2) {
        console.log('In frAddRow insertOnLoad');
        let REST_SERVICE_URI = './frAddRow/insertOnLoad';
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params2).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function getlistOfScreenParam(params) {
        var deferred = $q.defer();
        console.log('getlistOfScreenParam -------------------11')
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function editRow(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            console.log('Response Success' + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Saving params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteRow(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        var deferred = $q.defer();
        console.log('saveRow---------------------')
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);

        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }

    return {
        insertOnLoad: insertOnLoad,
        getlistOfScreenParam:getlistOfScreenParam,
        saveRow: saveRow,
        deleteRow: deleteRow,
        editRow: editRow,
        getRowId: getRowId

    };

}])

app.factory('T70Factory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_0 = './T70/insertOnLoad';
    var REST_SERVICE_URI_1 = './T70/getScreenParam';
    var REST_SERVICE_URI_2 = './T70/saveRow';
    var REST_SERVICE_URI_3 = './T70/deleteRow';
    var REST_SERVICE_URI_4 = './T70/editRow';
    var REST_SERVICE_URI_5 = './T70/getRowId';
    var REST_SERVICE_URI_6 = './T70/getlistOfDetails';

    function getRowId(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getlistOfDetails(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function insertOnLoad(params2) {
        console.log('In frAddRow insertOnLoad');
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params2).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function getlistOfScreenParam(params) {
        var deferred = $q.defer();
        console.log('getlistOfScreenParam -------------------11')
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function editRow(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            console.log('Response Success' + response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Saving params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function deleteRow(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function saveRow(params) {
        var deferred = $q.defer();
        console.log('saveRow---------------------')
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            console.log('Response Success')
            deferred.resolve(response.data);

        }, function (errResponse) {
            console.error('Error while fetching params');
            deferred.reject(errResponse);
        });
        return deferred.promise;

    }
    return {
        insertOnLoad: insertOnLoad,
        getlistOfScreenParam:getlistOfScreenParam,
        saveRow: saveRow,
        deleteRow: deleteRow,
        editRow: editRow,
        getRowId:getRowId,
        getlistOfDetails:getlistOfDetails

    };

}])


// Author V1012297
app.factory('WN09Factory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_0 = './WN09/insertOnLoad';
    var REST_SERVICE_URI_1 = './WN09/getScreenData';
    var REST_SERVICE_URI_2 = './WN09/saveData';
    function insertOnLoad(params) {
        console.log('In WN09Factory insertOnLoad');
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getScreenData(reportId) {
        console.log('In WN09Factory getScreenData ');
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getScreenData'+errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function saveData(params) {
        console.log('In WN09Factory updateField');
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getScreenData: getScreenData,
        saveData:saveData
    };
}])


app.factory('WN03Factory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_0 = './WN03/insertOnLoad';
    var REST_SERVICE_URI_1 = './WN03/getScreenData';
    var REST_SERVICE_URI_2 = './WN03/saveData';
    var REST_SERVICE_URI_3 = './WN03/SubmitData';
    function insertOnLoad(params) {
        console.log('In WN03Factory insertOnLoad');
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while insertOnLoad');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getScreenData(reportId) {
        console.log('In WN03Factory getScreenData ');
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, reportId).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getScreenData'+errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function saveData(params) {
        console.log('In WN03Factory updateField');
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateField');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function SubmitData(params) {
        console.log('In WN03Factory SubmitData');
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while SubmitData');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    return {
        insertOnLoad: insertOnLoad,
        getScreenData: getScreenData,
        saveData:saveData,
        SubmitData:SubmitData
    };
}])


app.factory('FRtoIrisFactory', ['$http', '$q', function ($http, $q) {

    var REST_SERVICE_URI_0 = './frReturn/downloadFiles';
    var REST_SERVICE_URI_1 = './frReturn/FRtoIRIS';


    function downloadFiles(params) {
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function FRtoIRIS(params2) {
            var deferred = $q.defer();
            $http.post(REST_SERVICE_URI_1, params2).then(function (response) {
                deferred.resolve(response.data);
            }, function (errResponse) {
                console.error('Error while FRtoIRIS');
                deferred.reject(errResponse);
            });
            return deferred.promise;
        }
    return {
        downloadFiles :downloadFiles,
        FRtoIRIS :FRtoIRIS

    };
}])

app.factory('CircleMappingFactory', ['$http', '$q', function ($http, $q) {

    var REST_URI_CIRCLE_CODE = "./CircleMapping/GetListOfCircleCode";
    var REST_URI_GET_BRANCH_DETAILS = "./CircleMapping/GetBranchDetails";
    var REST_URI_RAISE_REQUEST = "./CircleMapping/RaiseRequest";
    var factory = {
        getCircleCodeUs: getCircleCodeUs,
        getSearchFlag: getSearchFlag,
        raiseRequest: raiseRequest
    };
    return factory;

    function getCircleCodeUs(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_CIRCLE_CODE, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getSearchFlag(params) {

        var deferred = $q.defer();
        $http.post(REST_URI_GET_BRANCH_DETAILS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while searching branch code' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function raiseRequest(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_RAISE_REQUEST, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching code');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('ChkCircleMapFactory', ['$http', '$q', function ($http, $q) {

    var REST_URI_REQ_LIST = "./ChkCircleMap/GetListOfRequest";
    var REST_URI_ACCEPT = "./ChkCircleMap/AcceptRequest";
    var REST_URI_REJECT = "./ChkCircleMap/RejectRequest";

    var factory = {
        getRequestList: getRequestList,
        acceptRequest: acceptRequest,
        rejectRequest: rejectRequest
    };
    return factory;

    function getRequestList(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_REQ_LIST, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching requests');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function acceptRequest(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_ACCEPT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while accepting request');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function rejectRequest(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_REJECT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting request');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('GitcDashboardFactory', ['$http', '$q', function ($http, $q) {

    var REST_URI_MAX_VAL = "./FrtDash/GetMaxVal";
    var REST_URI_STS = "./FrtDash/GetStatus";
    var REST_URI_RELOAD = "./FrtDash/Reload";
    var REST_URI_ACTION = "./FrtDash/Action";

    return {
        getMaxVal: getMaxVal,
        getFileStatus: getFileStatus,
        reloadFunctionality: reloadFunctionality,
        showSummary: showSummary,
        actionSummary: actionSummary,
    };

    function getMaxVal(param) {
        var deferred = $q.defer();
        $http.post(REST_URI_MAX_VAL, param).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching status for frt/GITC dash');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }


    function getFileStatus(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_STS, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while fetching status for frt/GITC dash');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function reloadFunctionality(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_RELOAD, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Reloading for frt/GITC dash');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function acceptSummary(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_ACCEPT, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Reloading for frt/GITC dash');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function actionSummary(params) {
        var deferred = $q.defer();
        $http.post(REST_URI_ACTION, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Reloading for frt/GITC dash');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function showSummary(report) {
        var REST_SERVICE_URI_1 = './FrtDash/ShowSummary';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, report, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('downloadJrxmls2Factory', ['$http', '$q', function ($http, $q) {

    var factory = {
        getListOfReports2: getListOfReports2,
        viewReport: viewReport,
        viewReportCircle: viewReportCircle,
        downloadReport: downloadReport,
        getBrList: getBrList
    };

    return factory;

    function getBrList(user) {

        var REST_SERVICE_URI = './Admin/getBrList';

        var deferred = $q.defer();
        //console.log('Returned Id getBrList : ' + user.userId);

        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while gettng branchList');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getListOfReports2(user) {
        var REST_SERVICE_URI = './Admin/getListOfReports2';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, user).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/viewReportJrxml';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function viewReportCircle(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/viewReportJrxmlCircle';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function downloadReport(formData) {
        // alert(formData.report.status);
        var REST_SERVICE_URI = './Admin/downloadReport';
        var deferred = $q.defer();
        $http.post(REST_SERVICE_URI, formData, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            console.log(response.data);
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('ifrsUserWorklistFactory', ['$http', '$q', function ($http, $q) {
    let REST_SERVICE_URI = './IFRSUser/DashBoardDetails';
    let REST_SERVICE_URI_0 = './IFRSUser/IFRSUserReportList';
    let REST_SERVICE_URI_1 = './IFRSUser/viewReport';
    let REST_SERVICE_URI_2 = './IFRSUser/IFRSUserAccept';
    let REST_SERVICE_URI_3 = './IFRSUser/IFRSUserReject';
    let REST_SERVICE_URI_4 = './IFRSUser/downloadXLReport';
    let REST_SERVICE_URI_5 = './IFRSUser/downloadPdfReport';
    let REST_SERVICE_URI_6 = './IFRSUser/getWorklist';

    return {
        getIfrsListOfCircles: getIfrsListOfCircles,
        getIfrsUserReportList: getIfrsUserReportList,
        getIfrsUserWorklistReport: getIfrsUserWorklistReport,
        viewIFRSReport: viewIFRSReport,
        acceptIFRSUser: acceptIFRSUser,
        rejectIFRSUser: rejectIFRSUser,
        downloadXLReport: downloadXLReport,
        downloadPdfReport: downloadPdfReport,
    };
    function getIfrsListOfCircles(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getIfrsListOfCircles for IFRS user :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function getIfrsUserReportList(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting report list for IFRS user :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function viewIFRSReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function acceptIFRSUser(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while accepting report :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function rejectIFRSUser(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting report from IFRS User :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadXLReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadPdfReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function getIfrsUserWorklistReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getIfrsUserWorklistReport for IFRS user :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);


app.factory('ifrsWorklistFactory', ['$http', '$q', function ($http, $q) {
    let REST_SERVICE_URI_0 = './IFRS/getIfrsReportList';
    let REST_SERVICE_URI_1 = './IFRS/createIfrsReport';
    let REST_SERVICE_URI_2 = './IFRS/acceptReport';
    let REST_SERVICE_URI_3 = './IFRS/rejectReport';
    let REST_SERVICE_URI_4 = './IFRS/viewReport';
    let REST_SERVICE_URI_5 = './IFRS/IFRSUserAccept';
    let REST_SERVICE_URI_6 = './IFRS/IFRSUserReject';
    let REST_SERVICE_URI_7 = './IFRS/downloadXLReport';
    let REST_SERVICE_URI_8 = './IFRS/downloadPdfReport';

    return {
        getIfrsReportList: getIfrsReportList,
        createIFRSReport: createIFRSReport,
        acceptIFRSReport: acceptIFRSReport,
        rejectIFRSReport: rejectIFRSReport,
        viewIFRSReport: viewIFRSReport,
        acceptIFRSUser: acceptIFRSUser,
        rejectIFRSUser: rejectIFRSUser,
        downloadXLReport: downloadXLReport,
        downloadPdfReport: downloadPdfReport,
    };
    function getIfrsReportList(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting report list :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function createIFRSReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getting report list :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function acceptIFRSReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while accepting report :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function rejectIFRSReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting report :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function viewIFRSReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

    function acceptIFRSUser(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while accepting report :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function rejectIFRSUser(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while rejecting report from IFRS User :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadXLReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_7, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadPdfReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_8, params, {
            responseType: 'arraybuffer'
        }).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updating User');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('liabilitiesFactory', ['$http', '$q', function ($http, $q) {
    let REST_SERVICE_URI_0 = './IFRS/getColumnRowHeaders';
    let REST_SERVICE_URI_1 = './IFRS/getReportData';
    let REST_SERVICE_URI_2 = './IFRS/deleteRow';
    let REST_SERVICE_URI_3 = './IFRS/createRow';
    let REST_SERVICE_URI_4 = './IFRS/updateRow';
    let REST_SERVICE_URI_5 = './IFRS/submitReport';



    return {
        insertOnLoad: insertOnLoad,
        getRowDetails: getRowDetails,
        deleteRowDetails: deleteRowDetails,
        createRowDetails: createRowDetails,
        updateRowDetails: updateRowDetails,
        submitReport: submitReport,
    };

    function insertOnLoad(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while onLoad call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function getRowDetails(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while onLoad call response :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function deleteRowDetails(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while delete row call response :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function createRowDetails(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while create row call response :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function updateRowDetails(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while update row call response ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function submitReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while submitting report ' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
}]);

app.factory('liabilitiesMasterFactory', ['$http', '$q', function ($http, $q) {
    let REST_SERVICE_URI_0 = './IFRS/liabiltiyMaster/getDropDownList';
    let REST_SERVICE_URI_1 = './IFRS/liabiltiyMaster/createChangeRequest';
    let REST_SERVICE_URI_2 = './IFRS/liabiltiyMaster/updateGroupChangeRequest';
    let REST_SERVICE_URI_3 = './IFRS/liabiltiyMaster/delete';
    let REST_SERVICE_URI_4 = './IFRS/liabiltiyMaster/approve';
    let REST_SERVICE_URI_5 = './IFRS/liabiltiyMaster/reject';
    let REST_SERVICE_URI_6 = './IFRS/liabiltiyMaster/modifyPosition';

    return {
        getList: getList,
        createNewRow: createNewRow,
        updateRowData: updateRowData,
        deleteRowData: deleteRowData,
        approvePendingRequest: approvePendingRequest,
        rejectPendingRequest: rejectPendingRequest,
        saveColumnPositions: saveColumnPositions,
    };

    function getList(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getList call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function createNewRow(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while createNewRow call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function updateRowData(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while updateRowData call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function deleteRowData(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while deleteRowData call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function approvePendingRequest(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while deleteRowData call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function rejectPendingRequest(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_5, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while deleteRowData call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function saveColumnPositions(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_6, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while deleteRowData call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }

}]);

app.factory('IFRSArchivesFactory', ['$http', '$q', function ($http, $q) {
    let REST_SERVICE_URI_0 = './IFRSArchives/GetCircleList';
    let REST_SERVICE_URI_1 = './IFRSArchives/ArchiveReportsDownloadXL';
    let REST_SERVICE_URI_2 = './IFRSArchives/ArchiveReportsDownloadPDF';
    let REST_SERVICE_URI_3 = './IFRSArchives/ArchiveReportsDownloadPDFUser';
    let REST_SERVICE_URI_4 = './IFRSArchives/ArchiveReportsDownloadXLUser';

    return {
        getCircleList: getCircleList,
        downloadXLReport: downloadXLReport,
        downloadPdfReport: downloadPdfReport,
        downloadPdf: downloadPdf,
        downloadXL: downloadXL,
    };

    function getCircleList(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_0, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while getList call respponse :' + errResponse);
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadXLReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_1, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Downloading');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadPdfReport(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_2, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Downloading');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadPdf(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_3, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Downloading');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }
    function downloadXL(params) {
        let deferred = $q.defer();
        $http.post(REST_SERVICE_URI_4, params).then(function (response) {
            deferred.resolve(response.data);
        }, function (errResponse) {
            console.error('Error while Downloading');
            deferred.reject(errResponse);
        });
        return deferred.promise;
    }



}]);
