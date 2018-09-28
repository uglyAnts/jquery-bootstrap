# jquery-bootstrap
使用jquery + bootstrap开发的个人主页
{
  "name": "study-react",
  "version": "1.0.0",
  "description": "云视幻影融合版前端项目（基于React）",
  "main": "index.js",
  "scripts": {
    "start": "webpack-dev-server --progress --config ./build/webpack.config.development.js",
    "build": "webpack --config ./build/webpack.config.production.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "shiq",
  "license": "ISC",
  "devDependencies": {
    "autoprefixer": "^9.1.5",
    "babel": "^6.23.0",
    "babel-cli": "^6.26.0",
    "babel-loader": "^7.1.5",
    "babel-plugin-react-transform": "^3.0.0",
    "babel-preset-env": "^1.7.0",
    "babel-preset-react": "^6.24.1",
    "clean-webpack-plugin": "^0.1.19",
    "css-loader": "^1.0.0",
    "extract-text-webpack-plugin": "^4.0.0-beta.0",
    "html-webpack-plugin": "^3.2.0",
    "less": "^3.8.1",
    "less-loader": "^4.1.0",
    "postcss-loader": "^3.0.0",
    "react": "^16.5.2",
    "react-dom": "^16.5.2",
    "react-redux": "^5.0.7",
    "react-router-dom": "^4.3.1",
    "react-transform-hmr": "^1.0.4",
    "redux": "^4.0.0",
    "redux-thunk": "^2.3.0",
    "style-loader": "^0.23.0",
    "uglifyjs-webpack-plugin": "^2.0.1",
    "url-loader": "^1.1.1",
    "webpack": "^4.20.2",
    "webpack-cli": "^3.1.1",
    "webpack-dev-server": "^3.1.9",
    "webpack-merge": "^4.1.4"
  }
}



const path = require('path');
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextWebpackPlugin = require('extract-text-webpack-plugin');
const CleanWebpackPlugin = require('clean-webpack-plugin');

module.exports = {
	entry: {
		main: './src/index.jsx'
	},
	output: {
		path: path.resolve(__dirname, '../dist'),
		filename: '[name].bundle.js'
	},
	module: {
		rules: [
		    {
		    	test: /\.jsx?$/,
		    	use: {
		    		loader: 'babel-loader',
		    		options: {
		    			presets: ['env', 'react']
		    		}
		    	},
		    	exclude: /node_modules/
		    },
		    {
		    	test: /\.(c|le)ss$/,
		    	use: [
		    	    'style-loader',
		    	    'css-loader',
		    	    {
		    	    	loader: 'postcss-loader',
		    	    	options: {
		    	    		plugins: [
		    	    		    require('autoprefixer')({
		    	    		    	browsers: ['last 5 version']
		    	    		    })
		    	    	    ]
		    	    	}
		    	    },
		    	    {
		    	    	loader: 'less-loader'
		    	    }
		    	]
		    },
		    {
		    	test: /\.(png|jpg|jpeg|gif|woff|svg|eot|woff2|tff)$/,
		    	use: 'url-loader',
		    	exclude: /node_modules/
		    }
		]
	},
	plugins: [
	    new HtmlWebpackPlugin({
	    	filename: 'index.html',
	    	template: './src/index.html'
	    }),
	    new ExtractTextWebpackPlugin('style.css'),
	    new CleanWebpackPlugin(['dist']),
	    new webpack.HotModuleReplacementPlugin()
	]
};




const webpack = require('webpack');
const merge = require('webpack-merge');
const baseConfig = require('./webpack.config');

module.exports = merge(baseConfig, {
	mode: 'development',
	devtool: 'inline-source-map',
	plugins: [
	    new webpack.DefinePlugin({
	    	'process.env.NODE_ENV': JSON.stringify('development')
	    })
	],
	devServer: {
		inline: true,
		port: 8090,
		hot: true,
		open: true,
		proxy: {
			'/api': {
				target: '10.100.60.238:6065'
			}
		}
	}
});

const webpack = require('webpack');
const merge = require('webpack-merge');
const UglifyJsPlugin = require('uglifyjs-webpack-plugin');
const baseConfig = require('./webpack.config');

module.exports = merge(baseConfig, {
	mode: 'production',
	devtool: 'source-map',
	optimization: {
	    minimizer: [new UglifyJsPlugin({
	    	sourceMap: true,
	    	extractComments: true
	    })],
	    splitChunks: {
            cacheGroups: { 
                commons: {
                    name: "commons",
                    chunks: "all", 
                    minChunks: 2,
                    priority: 0 
                },
                vendor: { 
                    name: 'vendor',
                    test: /[\\/]node_modules[\\/]/,
                    chunks: 'all', 
                    priority: 10 
                }
            }
        }
	},
	plugins: [
	    new webpack.DefinePlugin({
	    	'process.env.NODE_ENV': JSON.stringify('production')
	    })
	]
});

{
    "presets": ["react", "env"],
    "env":
    {
        "development":
        {
            "plugins": [
                [
                    "react-transform",
                    {
                        "transforms": [
                        {
                            "transform": "react-transform-hmr",
                            "imports": ["react"],
                            "locals": ["module"]
                        }]
                    }
                ]
            ]
        }
    }
}
// .babelrc

