spec = load_oldest_ready_spec("./Specs/")

architect_anaita(spec)

is_complete = False

while not is_complete:
    developer_shahida(spec, TODO)
    evaluator_doner(spec, TODO)

    if TODO.todos_left == 0 and doner_approved:
        is_complete = nit_pick_kebab(spec, TODO)

write_final_report()  