# Assoc_mem_Japanese_cars
#Using Associative Memory objects to try and chunk semantics
# I am pasting 2 sets of code, the first creates assoc_mem modules for Toyota_models and Japanese_car_models.  The second adds an  Asian_car_making country module.  The modules are connected and for the first set a transform weight of 1 is sufficient to activate each module.  In the secondset of code, a transform weight of 1 is not able to activate the new Asian_car_making country module.
##############################
import nengo
import nengo.spa as spa
# use associative memory instead of state because holds more memories
# why arem't vocabs shown in gui?
# input_keys-> output_keys; hash table
import numpy as np
# note no noise 
# AM not fedback on self ...
# parsing local-global
D = 64
model = spa.SPA()
with model:
   model.Toyota_new_vocab_specific = spa.Vocabulary(D)
   model.Toyota_new_vocab_general = spa.Vocabulary(D)
  
   Toyota_new_vocab =  ['TOYOTA', 'RAV4', 'COROLLA', 'CAMRY']
   Toy_spec_gen_input_keys = ['RAV4', 'COROLLA', 'CAMRY']
   Toy_spec_gen_output_keys = ['TOYOTA', 'TOYOTA', 'TOYOTA']
   for word in Toyota_new_vocab:
       model.Toyota_new_vocab_general.parse(word)
   model.AM_Toyota_models_spec = spa.AssociativeMemory(input_vocab = model.Toyota_new_vocab_general, output_vocab = model.Toyota_new_vocab_general)
   
   model.AM_Toyota_general = spa.AssociativeMemory(input_vocab = model.Toyota_new_vocab_general, output_vocab = model.Toyota_new_vocab_general,
        input_keys = Toy_spec_gen_input_keys, output_keys =Toy_spec_gen_output_keys)
   def input_Toyota_model(t):
       if 0 < t < 3:
           return 'COROLLA'
       else:
           return '0'
   Japanese_spec_general_input_keys = ['TOYOTA', 'HONDA']
   Japanese_spec_general_output_keys = ['JAPANESE', 'JAPANESE']
   model.Input_Toyota_model = spa.Input(AM_Toyota_models_spec = input_Toyota_model)
   nengo.Connection(model.AM_Toyota_models_spec.output, model.AM_Toyota_general.input, synapse = 0.2, transform = 1, seed = 1)
   
   Japanese_car_vocab = ['JAPANESE', 'HONDA', 'TOYOTA']
   
   model.vocab_Japanese_car = spa.Vocabulary(D)
   for word in Japanese_car_vocab:
       model.vocab_Japanese_car.parse(word)
       
   model.AM_Japanese = spa.AssociativeMemory(input_vocab = model.Toyota_new_vocab_general, output_vocab = model.Toyota_new_vocab_general,
        input_keys = Japanese_spec_general_input_keys, output_keys = Japanese_spec_general_output_keys)
   nengo.Connection(model.AM_Toyota_general.output, model.AM_Japanese.input, synapse = 0.2, transform = 1, seed = 1)        
   
   
   ####################
   import nengo
import nengo.spa as spa
# use associative memory instead of state because holds more memories
# why arem't vocab shown in gui?
# are they considered somehow related to state or other 
# input_keys-> output_keys; hash table
import numpy as np
# note no noise 
# AM not fedback on self ...
# parsing local-global
D = 64
model = spa.SPA()
with model:
   model.Toyota_new_vocab_specific = spa.Vocabulary(D)
   model.Toyota_new_vocab_general = spa.Vocabulary(D)
  
   Toyota_new_vocab =  ['TOYOTA', 'RAV4', 'COROLLA', 'CAMRY']
   Toy_spec_gen_input_keys = ['RAV4', 'COROLLA', 'CAMRY']
   Toy_spec_gen_output_keys = ['TOYOTA', 'TOYOTA', 'TOYOTA']
   for word in Toyota_new_vocab:
       model.Toyota_new_vocab_general.parse(word)
   model.AM_Toyota_models_spec = spa.AssociativeMemory(input_vocab = model.Toyota_new_vocab_general, output_vocab = model.Toyota_new_vocab_general)
   
   model.AM_Toyota_general = spa.AssociativeMemory(input_vocab = model.Toyota_new_vocab_general, output_vocab = model.Toyota_new_vocab_general,
        input_keys = Toy_spec_gen_input_keys, output_keys =Toy_spec_gen_output_keys)
   def input_Toyota_model(t):
       if 0 < t < 3:
           return 'COROLLA'
       else:
           return '0'
   Japanese_spec_general_input_keys = ['TOYOTA', 'HONDA']
   Japanese_spec_general_output_keys = ['JAPANESE', 'JAPANESE']
   model.Input_Toyota_model = spa.Input(AM_Toyota_models_spec = input_Toyota_model)
   nengo.Connection(model.AM_Toyota_models_spec.output, model.AM_Toyota_general.input, synapse = 0.2, transform = 1, seed = 1)
   
   Japanese_car_vocab = ['JAPANESE', 'HONDA', 'TOYOTA']
   
   model.vocab_Japanese_car = spa.Vocabulary(D)
   for word in Japanese_car_vocab:
       model.vocab_Japanese_car.parse(word)
       
   model.AM_Japanese = spa.AssociativeMemory(input_vocab = model.Toyota_new_vocab_general, output_vocab = model.Toyota_new_vocab_general,
        input_keys = Japanese_spec_general_input_keys, output_keys = Japanese_spec_general_output_keys)
   nengo.Connection(model.AM_Toyota_general.output, model.AM_Japanese.input, synapse = 0.2, transform = 1, seed = 1)        
   
   vocab_Asian = ['JAPANESE', 'KOREAN', 'ASIAN']
   Asian_spec_general_input_keys = ['JAPANESE', 'KOREAN']
   Asian_spec_general_output_keys = ['ASIAN', 'ASIAN']
   
   model.vocab_Asian = spa.Vocabulary(D)
   for word in vocab_Asian:
       model.vocab_Asian.parse(word)
   model.AM_Asian = spa.AssociativeMemory(input_vocab = model.vocab_Asian, output_vocab = model.vocab_Asian,
        input_keys = Asian_spec_general_input_keys, output_keys = Asian_spec_general_output_keys)
   # it was not activated with transform = 1
   # nengo.Connection(model.AM_Japanese.output, model.AM_Asian.input, synapse = 0.2, transform = 1, seed = 1)        
   nengo.Connection(model.AM_Japanese.output, model.AM_Asian.input, synapse = 0.2, transform = 2, seed = 1)        
   
