A wrapper on top of [jquery-textcomplete](https://github.com/yuku-t/jquery-textcomplete), build for angularjs app

### Dependencies

* [jQuery](http://jquery.com) (>= 1.7.0) OR Zepto (>= 1.0)
* [AngularJS](http://angularjs.org) v1.0.1+


### Gettting started

jQuery MUST be loaded ahead.

```javascript
<script src="path/to/jquery.js"></script>
```

Include ng-textcomplete module script with AngularJS script on your page.

```javascript
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.1.5/angular.min.js"></script>
<script src="https://raw.github.com/fraserxu/ng-textcomplete/master/ng-textcomplete.js"></script>
```

Add textcomplete to your app module's dependency.

```
angular.module('myApp', ['ngTextcomplete'])

.directive('textcomplete', function(Textcomplete, $log, $rootScope) {
    return {
        restrict: 'EA',
        scope: {
            members: '='
        },
        template: '<textarea ng-model=\'message\' type=\'text\'></textarea>',
        link: function(scope, iElement, iAttrs) {

            var mentions = scope.members;
            var ta = iElement.find('textarea');
            var textcomplete = new Textcomplete(ta, {
                mention: {
                    match: /(^|\s)@(\w*)$/,
                    search: function(term, callback) {
                        callback($.map(mentions, function(mention) {
                            return mention.toLowerCase().indexOf(term.toLowerCase()) === 0 ? mention : null;
                        }));
                    },
                    index: 2,
                    replace: function(mention) {
                        return '$1@' + mention + ' ';
                    }
                }
            });

            scope.$watch('message', function(aft, bef) {
                $log.log('watch message', scope.message);
            })

            $rootScope.$on('onSelect', function(event, data) {
                scope.message = data;
                $log.log('select message', scope.message)
            })
        }
    }
});
```

And in your template, use it like this:
```
<textcomplete ng-model='members'></textcomplete>
```

### Install with Bower

Note that the ng-textcomplete bower package contains no AngularJS dependency.

```
# install AngularJS (stable)
bower install angular
# or (unstable)
bower install PatternConsulting/bower-angular

# install ng-textcomplete
bower install ng-textcomplete
```