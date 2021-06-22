

Twitter Bootstrap 4 Extension for Yii 2

This is the Twitter Bootstrap extension for Yii framework 2.0. It encapsulates Bootstrap 4 components and plugins in terms of Yii widgets, and thus makes using Bootstrap components/plugins in Yii applications extremely easy.

For license information check the LICENSE-file.

Documentation is at docs/guide/README.md.

Latest Stable Version Total Downloads Build Status

Installation
The preferred way to install this extension is through composer.

Either run

php composer.phar require --prefer-dist yiisoft/yii2-bootstrap4
or add

"yiisoft/yii2-bootstrap4": "~2.0.6"
to the require section of your composer.json file.

Usage
For example, the following single line of code in a view file would render a Bootstrap Progress plugin:

<?= yii\bootstrap4\Progress::widget(['percent' => 60, 'label' => 'test']) ?>
