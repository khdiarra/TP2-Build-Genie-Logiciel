require 'rake/clean'

CLEAN.include('*.o','exemple')

task :default => ["exemple"]

SRC = FileList['*.c']
OBJ = SRC.ext('o')

rule '.o' => '.c' do |t|
	sh "cc -c -o #{t.name} #{t.source}"
end

file "exemple" => OBJ do
	sh "cc -o exemple #{OBJ}"
end

#File dependencies go here
file 'main.o' => ['main.c', 'function.h']
file 'function.o' => ['function.c']

