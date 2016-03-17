# Fragment #

##  ##
    FragmentTransaction transaction = fm.beginTransaction();
	transaction.add();
	transaction.remove();
	transaction.replace();
	transaction.hide();
	transaction.show();

	transaction.addToBackStack();

	transaction.commit();