# Boolean Lang Parser

prog → begin stm* end

stm → ID := expr ;

| write ( ID ) ;

| read ( ID ) ;

expr → expr & expr

| expr | expr 

| ~ expr

| BV

| ID

Boolean values BV are either True or False

java file pattern

take inputFile and create FileInputStream

Create lexer using the inputStream

Get CommonTokensStream tokens from lexer

Pass tokens to Parser

Create tree from parser