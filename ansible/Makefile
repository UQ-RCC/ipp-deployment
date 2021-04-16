HEAT_TEMPLATE=heat.yaml

check-vars:
	test -n "$(SSH_KEY_NAME)" # $$SSH_KEY_NAME
	test -n "$(STACK_NAME)" # $$STACK_NAME
	test -n "$(OS_PROJECT_NAME)" # $$OS_PROJECT_NAME

stack: check-vars validate
	openstack stack create \
		-t ${HEAT_TEMPLATE} ${STACK_NAME}

update: check-vars validate
	openstack stack update -t ${HEAT_TEMPLATE} ${STACK_NAME}

${HEAT_TEMPLATE}: ${HEAT_TEMPLATE}.j2 gen-heat-template.py
	./gen-heat-template.py \
		--node-image-id=03433e1b-d092-40eb-bbce-cd3842899001 \
		--node-initial-user=ubuntu \
		--node-flavour=${NODE_FLAVOUR} \
		--ansible-user=ubuntu \
		--ssh-key-name=${SSH_KEY_NAME} \
		--avail-zone=QRIScloud \
		--project-name=${STACK_NAME} \
		--volume-size=${VOUME_SIZE} \
		--node-name=ipp-${STACK_NAME}

destroy-stack:
	openstack stack delete -y ${STACK_NAME}

validate: ${HEAT_TEMPLATE}
	openstack orchestration template validate -t ${HEAT_TEMPLATE} # > /dev/null

clean:
	rm -f inventory.yml ${HEAT_TEMPLATE}

inventory.yml: ${HEAT_TEMPLATE} make_inventory_openstack.py
	./make_inventory_openstack.py ${STACK_NAME} > $@

inventories: check-vars inventory.yml

# bootstrap: inventories bootstrap.yml
bootstrap: bootstrap.yml
	ansible-playbook -i inventory.yml bootstrap.yml

.PHONY: check-vars clean stack inventories bootstrap inventory.yml deploy-ipp
