## sass 应用

```

//Container
@mixin comp($_name){
    .#{$_name} {
        @content;
    }
}

//Container - repeated
@mixin repeated($_name){
    .#{$_name} {
        @content;
    }
}

//Container - main css properties
@mixin main {
    @content;
}

//Container - css properties for hover
@mixin state-hover ($_inverse: false) {
    @if ($_inverse == true){
        &:not(:hover){
            @content;
        }
    }@else{
        &:hover {
            @content;
        }
    }
}

//Container - css properties for active
@mixin state-active ($_inverse: false) {
    @if ($_inverse == true){
        &:not(.active), &:not(:focus) {
            @content;
        }
    }@else{
        &.active, &:focus, &:active {
            @content;
        }
    }
}

//Container - css properties for disable
@mixin state-disable ($_inverse: false) {
    @if ($_inverse == true){
        &:not(.disable){
            @content;
        }
    }@else{
        &.disable, &:disable {
            @content;
        }
    }
}

//Container - css properties for suffix class, such as 'primary' suffix in .eu-button.primary
@mixin modifier ($_names, $_inverse: false) {
    $_selector: '';

    @each $_name in $_names{
        $_selector: $_selector + '.' + $_name;
    }

    @if ($_inverse == true){
        @at-root &:not(#{$_selector}) { 
            @content; 
        }
    }@else{
        @at-root #{& + $_selector} {
            @content;
        }
    }
}

//Container - compatible css properties
@mixin compatible {
    @content;
}

//Container - css properties for small device
@mixin screen ($_min-width: null, $_max-width: null) {
    @if ($_min-width != null) and ($_max-width != null){
        @media only screen and (min-width: $_min-width) and (max-width: $_max-width - 1px) {
            @content;
        }
    }@else if ($_min-width != null) or ($_max-width == null){
        @media only screen and (min-width: $_min-width){
            @content;
        }
    }@else if($_min-width == null) or ($_max-width != null){
        @media only screen and (max-width: $_max-width - 1px){
            @content;
        }
    }
}

@mixin screen-small() {
    @include screen(null, $screen-small) {
        @content;
    }
}

@mixin screen-medium() {
    @include screen($screen-small, $screen-medium) {
        @content;
    }
}

@mixin screen-large() {
    @include screen($screen-medium, $screen-large) {
        @content;
    }
}

@mixin screen-extra-large() {
    @include screen($screen-large) {
        @content
    }
}

/*------------------------------   Property   ------------------------------*/

/*
 * Add single-line setup to the element
 */
@mixin single-line {
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow-x: hidden;
}

/*
 * Add prefix to the specific CSS property
 * @param {String} $_prop - String of the CSS property
 * @param {Value} $_value - Value for the property
 * @param {List of String} $_prefixex - List of prefixes which is added to the property 
 */
@mixin prefixer ($_prop,$_value, $_prefixes) {
    @each $_prefix in $_prefixes{
        -#{$_prefix}-#{$_prop}: $_value;
    }
    #{$_prop}: $_value;
}

/*
 * Add property which can be seperated to four sides' properties to the element
 * @param {String} $_prop - String of the CSS property
 * @param {List} $_value - Value for the property 
 * @param {String} $_suffix - String to suffix which is added to the property
 * @param {List} $_sub-props
 */
@mixin space ($_prop,$_value:null,$_suffix:null,$_sub-props:top right bottom left) {
    $_length:min(length($_value), length($_sub-props));
    @for $_i from 1 to $_length+1 {
        #{$_prop+if($_sub-props,'-'+nth($_sub-props,$_i),'')+if($_suffix,'-'+$_suffix,'')}: nth($_value,$_i);
    }
}

/**
 * Add divider to the element
 * @param {Map of property paring with value} $_setting - Map for adding the extra properties to the divider
 * @param {Value} $_length - Value of divider's length
 * @param {Boolean} $_verticaled - Boolean for which the divider is vertical or not
 */
@mixin divider ($_setting:null,$_length:null,$_verticaled:false) {
    @if(not $_setting){$_setting:()}
    @if(not map-has-key($_setting,top)){$_setting:map-merge($_setting,(top:0))}
    @if(not map-has-key($_setting,left)){$_setting:map-merge($_setting,(left:0))}
    @if(not map-has-key($_setting,border-color)){$_setting:map-merge($_setting,(border-color: $color-grey-lighter))}
    position: relative;
    &::before{
        display: block;
        background-color: transparent;
        border-style: solid;
        border-width: 0;
        position: absolute;
        content: " ";
        @if($_verticaled){
            border-left-width: 1px;
            width: 1px!important;
            height: if($_length,$_length,calc( 100% - #{$padding*2}));
        }@else{
            border-bottom-width: 1px;        
            height: 1px;
            width: if($_length,$_length,100%);
        }
        @each $_prop, $_value in $_setting{
            #{$_prop}:$_value;
        }
    }
}

/*
 * Setup the element's z-index
 * @param {Value} $_layer - the key of the map which contains z-index value
 * @param {Value} $_position - Value of position
 */
@mixin z-index ($_layer:base,$_position:null) {
    $_layer:map-get($layer,$_layer);
    position:$_position;
    z-index: $_layer;
}

/** 
 * Setup the element's border
 * @param {Value|List of Value} $_width - Value of border-width | List of value for four sides' border-width
 * @param {Value|List of Value|String} $_color - Value of border-color | List of value for four sides' border-color
 * @param {Value|List of Value} $_style - Value of border-style | List of value for four sides' border-style
 * @param {String} $_suffix - set the property to the specific side's border
 */
@mixin border ($_width,$_color:null,$_style:solid, $_suffix:null) {
    $_sub-props:top right bottom left;
    @if(length($_width)>1){
        @include space(border,$_width,width,$_sub-props);
    }@else{
        border#{if($_suffix,'-'+$_suffix,'')}-width: $_width;
    }
    @if(length($_color)>1){
        @include space(border,$_color,color,$_sub-props);
    }@else{
        border#{if($_suffix,'-'+$_suffix,'')}-color: $_color;
    }
    @if(length($_style)>1){
        @include space(border,$_style,style,$_sub-props);
    }@else{
        border#{if($_suffix,'-'+$_suffix,'')}-style: $_style;
    }
}

/* 
 * Setup the element as flex box
 * @param {Value} $_direction - Value of flex-direction
 * @param {Value} $_wrap - Value of flex-wrap
 * @param {Value} $_justify - Value of justify-content
 * @param {Value} $_a-item - Value of align-items
 * @param {Value} $_a-content - Value of align-content
 */
@mixin flex ($_direction:null,$_wrap:null,$_justify:null,$_a-item:null,$_a-content:null) {
    display: -webkit-box;
    display: -ms-flexbox;
    display: -webkit-flex;
    display: flex;
    @include prefixer(flex-direction,$_direction,$prefix);
    @include prefixer(flex-wrap,$_wrap,$prefix);
    @include prefixer(justify-content,$_justify,$prefix);
    @include prefixer(align-items,$_a-item,$prefix);
    @include prefixer(align-content,$_a-content,$prefix);
}

/* 
 * Setup the element's flex item properties. If $_grow and $_shrink are 0, add min-width:0
 * @param {Value} $_grow - Value of flex-grow
 * @param {Value} $_shrink - Value of flex-shrink
 * @param {Value} $_order
 * @param {Value} $_basis - Value of flex-basis
 * @param {Value} $_align - Value of align-self
 */
@mixin flex-item ($_grow:0,$_shrink:1, $_order:null, $_basis:null,$_align:null) {
    @include prefixer(flex-grow,$_grow,webkit);
    @include prefixer(flex-shrink,$_shrink,webkit);
    @include prefixer(order,$_order,webkit);
    @include prefixer(flex-basis,$_basis,$prefix);
    @include prefixer(align-self,$_align,$prefix);
    @if($_grow == 1 and $_shrink == 1){
        min-width: 0;
    }
}

/*
 * Setup the element's font properties
 * @param {Value} $_size - Value of font-size
 * @param {Value} $_height - Value of line-height
 * @param {Value} $_color - Value of color
 * @param {Value} $_weight - Value of font-weight
 * @param {String} $_family - String of font-family, can be multi values
 */
@mixin font ($_size,$_height:null,$_color:null,$_weight:null,$_family...) {
    @if(length($_family)==0){$_family:null};
    font-size:$_size;
    line-height: $_height;
    color:$_color;
    font-weight: $_weight;
    font-family: $_family;
}

/**
 * Setup the element's position
 * @param {List of Value|String} $_value - List of top, right, bottom, left | String of 'reset' set the value of list to auto
 * @param {Value} $_position - Value of position
 * @param {Value} $_z-index - Value of z-index
 */
@mixin position ($_value,$_position:null,$_z-index:null) {
    $_sub-props:top right bottom left;
    @if length($_value) == 1 and $_value == 'reset'{$_value:auto auto auto auto}
    @include z-index($_z-index,$_position);
    @for $_i from 1 to length($_value)+1{
        #{nth($_sub-props,$_i)}:nth($_value,$_i);
    }
}

/** 
 * Setup the element's shape
 * @param {Value|List of Value} $_width - the Value of width | the List of height, min-height, and max-height
 * @param {Value|List of Value|String} $_height - Value of height | List of height, min-height, and max-height | String of shape type : 'rect', 'circle'
 * @param {Value|List of Value} $_radius - Value of border-radius | List of four corners
 */
@mixin shape ($_width,$_height,$_radius:null) {
    $_size-properties:'' 'min-' 'max-';
    $_radius-properties:if(length($_radius) > 1,'top-left' 'top-right' 'bottom-right' 'bottom-left',null);
    @if length($_height) == 1 and $_height == 'rect' {
        $_height: $_width;
    }
    @if length($_height) == 1 and $_height == 'circle' {
        $_height: $_width;
        @for $_i from 1 to length($_width)+1{
            @if nth($_width,$_i) {$_radius: nth($_width,$_i)/2;}
        }
    }
    @for $_i from 1 to length($_width)+1{
        #{nth($_size-properties,$_i)}width:nth($_width,$_i);
    }
    @for $_i from 1 to length($_height)+1{
        #{nth($_size-properties,$_i)}height:nth($_height,$_i);
    }
    @include space(border,$_radius,radius,$_radius-properties);
}

@mixin hardware ($_will-change:auto) {
    $_will-change:listToCommaValue($_will-change);
    will-change:$_will-change;
    perspective: 1000;
}

/**
 * Add transition setup to the element
 */
@mixin transition ($_prop,$_duration:.1s,$_function:ease,$_delay:null) {
    $_prop:listToCommaValue($_prop);
    $_duration:listToCommaValue($_duration);
    $_function:listToCommaValue($_function);
    $_delay:listToCommaValue($_delay);
    @include prefixer(transition-property,$_prop,$prefix);
    @include prefixer(transition-duration,$_duration,$prefix);
    @include prefixer(transition-timing-function,$_function,$prefix);
    @include prefixer(transition-delay,$_delay,$prefix);
}

/*------------------------------   Selector   ------------------------------*/

/**
 * Add selectors of column
 * @param {Number} $_col-num - Number of columns' number
 * @param {?} $_width
 * @param {String} $_suffix - String of suffix which is added to the end of selector
 */
@mixin column ($_col-num:if(variable-exists(col-num),$col-num,12), $_width: 100%, $_suffix:null) {
    $_unit-width: $_width / $_col-num;
    @for $_i from 1 through $_col-num{
        .eu-col#{if($_suffix,'-'+$_suffix,'')}-#{$_i}{
            width:#{$_unit-width * $_i}!important;
            /*min-width:#{$_unit-width * $_i}!important;*/
        }
    }
}

/**
 * Add selector of icon
 * @param {String} $_selector - String of the selector's name
 * @param {String} $_unicode - String of the unicode
 */
@mixin icon ($_selector,$_unicode) {
    $_unicode: unquote("\"\\#{$_unicode}\"");
    #{"." + $_selector}::before{
        font-family: 'Icons';
        font-style: normal;
        content:$_unicode;
    }
}

/**
 * @param {String} $_name
 * @param {Map} $_frames
 * @param {String} $_enable-reveerse
 * @param {Boolean|Map} $_create-selector
 */
@mixin animation ($_name, $_frames, $_enable-reverse:false, $_create-selector:true) {
    $_states: $_enable-reverse;
    $_states-frames-props:append((),$_frames);
    @if $_enable-reverse == true {
        $_map: ();
        $_keys: map-keys($_frames);
        $_values: map-values($_frames);
        $_length: length($_keys) + 1;
        @for $_i from 1 to $_length {
            $_j: $_length -$_i;
            $_key:nth($_keys,$_i);
            $_value:nth($_values,$_j);
            $_pair:($_key:$_value);
            $_map:map-merge($_map,$_pair);
        }
        $_states:"show" "hide";
        $_states-frames-props:append($_states-frames-props,$_map)
    }
    @if $_create-selector {
        $_selectors: ();
        $_name-selector:'.eu-animation-'+$_name;
        $_props:nth($_states-frames-props,1);
        $_props:map-get($_props,nth(map-keys($_props),1));
        $_props:map-keys($_props);
        @for $_i from 1 to length($_states)+1 {
            $_state: nth($_states,$_i);
            $_state-selector:if($_state,'-'+$_state,'');
            $_selectors: append($_selectors,($_name-selector + $_state-selector));
            $_animation-name:$_name + if($_state,'-'+$_state,'');
            $_animation-name:unquote($_animation-name);
            #{$_name-selector + $_state-selector + '.play'}{
                @include prefixer(animation-name,$_animation-name,$prefix);
            }
        }
        #{listToCommaValue($_selectors)}{
            @include hardware($_props);
            @if type-of($_create-selector) == "map" {
                @each $_prop, $_value in $_create-selector{
                    #{$_prop}: $_value;
                }
            }
        }
    }
    @for $_i from 1 to length($_states)+1 {
        $_state: nth($_states,$_i);
        $_frames-props:nth($_states-frames-props,$_i);
        $_animation-name:$_name + if($_state,'-'+$_state,'');
        @keyframes #{$_animation-name} {
            @each $_frame, $_frame-props in $_frames-props{
                #{$_frame} {
                    @each $_prop, $_value in $_frame-props{
                        #{$_prop}: $_value;
                    }
                }
            }
        }
    }
}

@mixin loading () {
    position: relative;
    &::before, &::after{
        @include shape(0.9em,circle);
        @include border(2px);
        display: block;
        content: ' ';
    }
    &::before{
        border-color: $color-grey-lighter;
    }
    &::after{
        border-color: $color-grey transparent transparent;
        position: absolute;
        top: 0;
        animation: mixin-loading 0.6s linear;
        animation-iteration-count: infinite;
    }
    @keyframes mixin-loading {
        from {
            -webkit-transform: rotate(0deg);
            transform: rotate(0deg);
        }
        to {
            -webkit-transform: rotate(360deg);
            transform: rotate(360deg);
        }
    }
}

```